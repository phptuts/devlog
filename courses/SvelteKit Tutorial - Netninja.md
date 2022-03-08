# SvelteKit Tutorial - Netninja

## Routes

So are svelte components that follow the folder structure.  What this means is that if you want an about page, you create a component in the src -> routes folder called about.svelte.

## Shared Components

Shared components should be stored in the lib folder, becuase we can use the $lib syntax to import them in.

### Title Component (Shared)

src -> lib -> title.svelte

```svelte
<script>
  export let title = 'Ninja Gaming';
</script>

<div class="title">
  <img src="/controller.svg" alt="site log" />
  <h1>{title}</h1>
</div>
```

### Used Component

src -> lib -> about.svelte

```svelte
<script>
  import Title from '$lib/title.svelte';
</script>

<Title title="Ninja Gaming Reviews - About" />

```

## Layouts

Layouts are found files that reset the whole the page.  The slot is where the page components html & css are renders. 
What is cool is that you import css directly into the layout.  Layout file names are __layouts.svelte


src -> lib -> __layout.svelte

```svelte
<script>
  import '../../styles/global.css';
  import Title from '$lib/title.svelte';
</script>

<header>
  <nav>
    <Title title="Ninja Gaming Guide" />
    <div class="links">
      <a href="/">Home</a>
      <a href="/about">About</a>
      <a href="/guides">Guides</a>
    </div>
  </nav>
</header>

<main>
  <slot />
</main>

<footer>
  <p>Copyright 2022 Ninja Gaming Guides</p>
</footer>

<style>
  header {
    text-align: center;
    background-color: rgba(0, 0, 0, 0.1);
  }

  main {
    max-width: 960px;
    margin: 20px auto;
  }
  footer {
    text-align: center;
  }
  .links {
    margin-left: auto;
  }
  a {
    margin-left: 10px;
  }
  nav {
    display: flex;
    align-items: center;
    max-width: 960px;
    margin: 0 auto;
  }
</style>
```

## Reset Layouts

These allow you to reset the layout for any folder of routes.  It's name is __layout.reset.svelte.

## SSR (Server Side Rendering)

This is done in script tag with the context="module" script.  It's done in an async function called load.  You can then pass props into the regular script tag.

You can also return a status and error.  You can also do a redirect as well.

```js
<script context="module">
  export async function load({ fetch, params }) {
    const response = await fetch(`/guides/${params['id']}.json`);
    const { guide } = await response.json();
    console.log(guide);
    if (response.ok) {
      return {
        props: {
          guide,
        },
      };
    }

    return {
      status: response.status,
      error: new Error('Guide not found.'),
    };
  }
</script>
<script>
  export let guide;
</script>
```

## Routing

You can add a wild card route by using the [id].svelte

In this case you would get params in your handler that would allow you access the id.

```svelte
<script context="module">
  export async function load({ fetch, params }) {
    const response = await fetch(`/guides/${params['id']}.json`);
   // ...
   }
<script>
```

## Prefetch

Prefetch is done by adding sveltekit:prefetch to anchor tag.

```svelte
      <li><a sveltekit:prefetch href="/guides/{guide.id}">{guide.title}</a></li>
```

## API Routes

They start with .js or you can put the extension by saying json.js.  

```js
export async function get({ params }) {
  const guides = [
    { id: 1, title: 'Some title 1', body: 'body' },
    { id: 2, title: 'Some title 2', body: 'body' },
    { id: 3, title: 'Some title 3', body: 'body' },
    { id: 4, title: 'Some title 4', body: 'body' },
    { id: 5, title: 'Some title 5', body: 'body' },
  ];

  const guide = guides.find((g) => g.id == params.id);

  if (guide) {
    return {
      status: 200,
      body: { guide },
    };
  }

  return {
    status: 404,
    error: new Error('Not found'),
  };
}

```
