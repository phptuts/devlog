# PostGIS In Action v3

This is my journey through the book [postgis in action](https://www.manning.com/books/postgis-in-action-third-edition).

## Day 1 - 2022-01-05

Today I only got through a few pages.  I learned a lot more about postgres than I did about postgis which is great.

- I learned serial means auto increment
- I learned that gist in general is the preferred index for spatial indexes
- I learend how to add a foreign key in postgres

I only got through 5 pages in 20 minutes but that's ok.  All things worth while take time.  Even 5 pages a day means that I will get through the book in a 112 which is not bad!!

```sql

-- create an populate the table lu_franchises

create table ch01.lu_franchises (id char(3) primary key, franchise varchar(30));

insert into ch01.lu_franchises (id, franchise) VALUES
('BKG', 'Burger King'), ('CJR', 'Carl''s Jr'),
('HDE', 'Hardee'), ('INO', 'In-N-Out'),
('JIB', 'Jack in the Box'), ('KFC', 'Kentucky Fried Chicken'),
('MCD', 'McDonald'), ('PZH', 'Pizza Hut'),
('TCB', 'Taco Bell'), ('WDY', 'Wendys');


-- create an populate the table restaurants
-- In postgres serial is the same as auto increment
create table ch01.restaurants (
id serial primary key, 
franchise char(3) not null,
geom geometry(point, 2163) -- This second is the SRID which identifies the type of map
)

-- This adds a spatial index to queries to make them faster
-- There are three kinds of spatial index but gist is usually the one you want to use.

create index ix_code_restuarant_geom on ch01.restaurants  using gist(geom);

-- Creates a foriegn key constraint

alter table ch01.restaurants 
add constraint fk_restuarant_lu_franchises
foreign key (franchise)
references ch01.lu_franchises (id)
on update cascade on delete restrict;

-- puts an index on the foriegn key

create index fi_restaurant_franchise on ch01.restaurants  (franchise)

-- creates the highway table
-- varying is the same as varchar
-- 
create table ch01.highways (
gid integer not null,
feature character varying(80),
name character varying(120),
state character varying(2),
geom geometry (multilinestring, 2163), -- notice we are using multi lines and not points
constraint pk_highways primary key (gid)
)

-- puts the index on the multi line string
create index ix_highways on ch01.highways using gist(geom);
```

## Day 2 - 2022-01-06

So I did not get far in the book, but I did a little about psql command and how to load data via a CSV.

### Creating the table and looking at the data to be imported

First, I create the staging table to load the data.

```sql
CREATE TABLE ch01.restaurants_staging (
          franchise text, lat double precision, lon double precision);

```

I then looked at the data.  BKG is for burger king, and the other two pieces of data are lat lngs.

```
BKG,25.8092,-80.24
BKG,25.8587,-80.2094
BKG,25.8471,-80.2415
BKG,25.8306,-80.2661
```

### Setting up psql command prompt on mac

I then worked on loading the data into Postgres.  When running the Postgres command psql, you will want to add it to your path.  On Mac, that means going to your .zshrc file and adding this.

```bash
export PATH=/Applications/Postgres.app/Contents/Versions/latest/bin/:$PATH
```

Be sure to source your .zshrc file once your done.

### Command to copy data

Then to run the command to upload the data using this.


```bash
psql -d postgis_in_action -c "\copy ch01.restaurants_staging from 'restaurants.csv' DELIMITER as ','";
```

### What it means

 - -c is for running the command
 - -d is for telling Postgres which database
 - ch01 is the Postgres schema
 - restaurants_staging is the table
 - restaurants.csv is the CSV file
 - DELIMITER is the character used to separate the data; in our case, it's a comma.

## Day 3 - 2022-01-07

This was another brutal day for PostGIS.  It took me forever, but I figured how to import shp / shapefile into the database.  I have to use a program called QGIS.  The learning curve is intense, but I am getting it.  I figured out adding a connection and importing the table into the database.  This would make a great tutorial, lol!


https://user-images.githubusercontent.com/9620015/148637444-c7024d28-93a2-436f-9f5a-05754920a86c.mov

## Day 4 - 2022-01-08

Today was a short day for PostGIS.  I learned/reviewed a few queries.  I was able to get all the data into the "productions" tables.  I then ran a query in the book. 

A few notes about this query

- We have to use distinct because a restaurant might intersect at two roads
- We are joining on distance with the ST_DWithing.  This means that if the restaurant or road geometry is within 1609 meters, consider it hit.
- The rest of this is standard SQL

So what is remarkable about this is that we can say if polygon A and polygon B are within x meters of each other join on it.  We could also do a where clause as well.  This is stupidly powerful!

```sql
SELECT f.franchise
 , COUNT(DISTINCT r.id) As total
FROM ch01.restaurants As r
  INNER JOIN ch01.lu_franchises As f ON r.franchise = f.id
INNER JOIN ch01.highways As h
      ON ST_DWithin(r.geom, h.geom, 1609)
GROUP BY f.franchise
ORDER BY total DESC;
```
<img width="318" alt="db" src="https://user-images.githubusercontent.com/9620015/148676502-a7c71799-7f55-428d-ba13-8981acea6b42.png">

## Day 5 - 2022-01-16

Today I worked on transfering the postgis action work to my local machine.  To do this I used docker.  I finally got everything working.  I also had to install postgis and postgis_raster on the public schema.  This book suggests against that but stackover says different.  Regardless it will work.  Bottom line I have a lot to learn about devops!

I also got QGIS and OpenJump install.  Both GIS applications required to do this book.  I am grateful I am doing alernate days!


### Docker

These must be in the same folder.

```Dockerfile
FROM postgres:13

RUN apt update
RUN apt install postgis postgresql-13-postgis-3-scripts --yes
```

```yml
version: '3.5'

services:
  postgres:
    container_name: postgres_container
    build: .
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    ports:
      - '5432:5432'
    restart: unless-stopped
```
### Installing Postgis


```sql
CREATE EXTENSION postgis;
CREATE EXTENSION postgis_raster;
```
