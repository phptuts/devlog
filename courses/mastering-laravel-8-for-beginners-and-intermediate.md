# Mastering Laravel 8 for Beginners & Intermediate

These are my notes from the course.  Not from the beginning of the course because I did not take notes from the beginning. 

## [course](https://www.udemy.com/share/101XFY3@c6l6ZJUGBGWXCct9Li6YPa7c1go5sgdaO8jAhi1DZF7EUgGzLyqOuHn3Ka_Si_8=/)

## [Code](https://github.com/phptuts/laravel-udemy)

## Day 1 - 2021-12-29

Today the teacher went over responses, middleware, and some of the laravel short methods.  Stuff like how to change the headers of a request.

## Day 2 - 2021-12-30

Today I in the Udemy course I learned how to create controller routes.  I did learn that you can create a single controller route.  I also learned more about laravel sails.  I can't publish the repo because of my job for now.

I also learned more about laravel sails.

### DB Beaver Setup with Laravel Sails

Setup is really easy with db beaver.  I need to go to driver properties.  

I had to set **allowPublicKeyRetrieval** to **true** and **useSSL** to **false**.

![Screen Shot 2021-12-30 at 3 17 46 PM](https://user-images.githubusercontent.com/9620015/147793890-327fa0d2-251d-4f20-8fe4-8d705f14ce3d.png)

I was also able to setup on mac an alias for sails by creating a ~/.zshrc file.

### Sails alias

1\. Run the command to create or edit the .zshrc
```bash
vi ~/.zshrc
```

2\. Copy this into the file

```
alias sail="bash ./vendor/bin/sail"
```

3\. Source the file

```
source ~/.zshrc
```

### Sails info

Every time you want to use artisan put sail in front of it.

```bash
sail artisan migrate
```

To start up the container run

```bash
sail up -d
```

To stop the container run

```bash
sail stop
```

## Day 3 - 2021-12-31

Today we went over database migrations and Eloquent models in Tinker.

I did learn that you can pass multiple ids in the static find function and it will return a collection of values.


```php
BlogPost::find([1,3,4]);
```

If 3 and 4 exist in the database and 1 does not it wil return a collection of 2 BlogPost.

## Day 4 - 2022-01-01

Today was not super productive with laravel.  I was using my work laptop and I discovered a few things. 

1\. I have php 7.4 install locally and I need to use php 8.1 for this version of laravel.  I was able to that doing this and running source on that file.  It makes it easy to switch back and forth.

```config
PHP_VERSION=7.4
export PATH="/usr/local/opt/php@$PHP_VERSION/bin:$PATH"
~                                                                               
```

2\. I also realized that for sails I had to use mysql has the host / internal url.  This took me a few hours to figure out out lol.  Luckily I had taken notes for db beaver which really saved me.  This is only locally for my tutorial.

```config
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=secret
```

3\. I realized that docker and vs code now can work together.  This means you can open up a container directly in vs code.  This is amazing!!!  

<img width="1263" alt="vs code" src="https://user-images.githubusercontent.com/9620015/147868280-9c1b681e-512a-47dd-9c50-b87a09fbb0c2.png">

I need to read more on this here:

https://code.visualstudio.com/docs/remote/containers

## Day 5 - 2022-01-02

Today we went the basics of laravel forms.  I learned how to create a form request which is a request that has already been validated.  I also learned that you can call make and create on laravel methods.  Make will not save to the database while create will.  In order to use the library you must specify a fillable property as an array of fields names that you can do a mass assignment with.

## Day 6 - 2022-01-04

We started on Laravel UI. I learned how to install it and how to configure it bootstrap.

## Day 7 - 2022-01-05

Today I learned about laravel mix.  Which is used to simplify webpack.  It's pretty neat.  I had to upgrade to laravel mix version 6 to get everything to work.

```bash
npm install laravel-mix@latest
```

Some cool features is that it does asset version right out of the box.  This means that your css and js have a unique hash attached to them so that when you deploy you get a new version.

Notice I am using the mix helper and not asset.

```html
    <link rel="stylesheet" href="{{ mix('css/app.css') }}">
    <script src="{{ mix('js/app.js') }}" defer></script>
```

```js
mix.js("resources/js/app.js", "public/js")
    .sass("resources/sass/app.scss", "public/css")
    .sourceMaps();

// this does the versioning.
if (mix.inProduction()) {
    mix.version();
}
```

## Day 8 - 2022-01-06

Today I learned about flex and not a lot about laravel.  I learned about positioning items up and down in a flexbox.  I also figured out a bug in the tutorial I was using.  It wasn't too bad.  I wanted my flexbox to have the links on the right and the title on the left, so I added the class `justify-content-between`.

Here is a handy chart I found on the bootstrap website.

https://getbootstrap.com/docs/4.0/utilities/flex/#justify-content

I know my tutorials have bugs as well.  I think it can be a good learning experience.  LOL


![Screen Shot 2022-01-06 at 11 12 27 AM](https://user-images.githubusercontent.com/9620015/148438080-e34c4514-2905-4bd4-89bd-7afab90c7ee4.png)

## Day 9 - 2022-01-07

Today the course went over styling which was good to know.  It was a review of the bootstrap classes.   It then went over testing.

## Day 10 - 2022-01-08

Today was a quick laravel day.  I learned how to run tests with laravel sails


```bash
sail test
```

I wanted to write down the basic framework for writing all tests.

- Arrange -> Setup
- Act -> Doing something
- Assert -> Verifying that everything is good

Example

```php
public function testSee1BlogPostWhenThereIs1()
    {
        // Arrange
        $post = new BlogPost();
        $post->title = "New Title";
        $post->content = "Hello World";
        $post->save();

        // Act
        $response = $this->get('/posts');

        // Assert
        $response->assertSeeText('New Title');

        $this->assertDatabaseHas('blog_posts', [
            'title' => 'New Title',
        ]);
    }
}
```

I also learned a new assert in Laravel.  The first argument is the table name and the second on is an array that creates the where clause.

```php
$this->assertDatabaseHas('blog_posts', [
            'title' => 'New Title',
        ]);
```

## Day 11 - 2022-01-09

Today we went over more testing.  Things like how to make a request.  To laravel credit testing end to end is really easy.  

```php
 $params = [
    'title' => 'x',
    'content' => 'x',
];
// They have these for every method of php
$this->post('/posts', $params)
    ->assertStatus(302)
    ->assertSessionHas('errors');
```

I also learned about some of the helper methods for testing.

```php
$response = $this->post('/posts', $params);

// can check messages in the session.  This would check if session key status key stores the word "test"
$response->assertSessionHas('test', session('status')); 

// Checks in the blog_posts table if a record exists with the title "New Title"
$this->assertDatabaseHas('blog_posts', [
            'title' => 'New Title',
        ]); 
        
// Checks in the blog_posts table if something does not exist in the database
$this->assertDatabaseMissing('blog_posts', [
            'title' => 'New Title',
            'content' => "Hello World",
        ]);
```

## Day 12 - 2022-01-16

Today I learned about 1 to 1 relationships in Laravel.  Say that you have an Author and a Profile.  The profile will have author_id field on it.

### Migration

#### Author

```php
Schema::create('authors', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
});
```

#### Profile

The unique constraint prevents us from an author having more than one profile. 

```php
Schema::create('profiles', function (Blueprint $table) {
            $table->id();
            $table->timestamps();

            $table->unsignedBigInteger('author_id')->unique();
            $table
                ->foreign('author_id')
                ->references('id')
                ->on('authors');
});
```

### Code

```php
class Author extends Model
{
    public function profile()
    {
        return $this->hasOne(Profile::class);
    }
}

```

```php
class Profile extends Model
{
    public function author()
    {
        return $this->belongsTo(Author::class);
    }
}
```

### Creating 

```php
// Option 1
$author = Author::create();
$profile = new Profile();
$author->profile()->save($profile);

// Option 2
$author = Author::create();
$profile->author()->associate($author)->save();

// Option 3
$author = Author::create();
$profile = new Profile();
$profile->author_id = $author->id;
$profile->save();
```

## Day 13 - 2022-01-18

Today I learned that laravel does lazy loading by default.  I also learned that you do eager loading with the "with" method.

```php
$author = Author::with('profile')->whereKey(1)->first();
```

I also learned about the whereKey which is the same as where('id', 3) expect whereKey will work when the primary key is not id.  This is find under the hood.

```php
// These two lines are the same.
$author = Author::with('profile')->whereKey(1)->first();
$author = Author::find(1);
```
