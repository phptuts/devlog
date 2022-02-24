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
