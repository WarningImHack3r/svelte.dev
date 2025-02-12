---
title: Layout data
path: /blog
---

Just as `+layout.svelte` files create UI for every child route, `+layout.server.js` files load data for every child route.

Suppose we'd like to add a 'more posts' sidebar to our blog post page. We _could_ return `summaries` from the `load` function in `src/routes/blog/[slug]/+page.server.js`, like we do in `src/routes/blog/+page.server.js`, but that would be repetitive.

Instead, let's rename `src/routes/blog/+page.server.js` to `src/routes/blog/+layout.server.js`. Notice that the `/blog` route continues to work — `data.summaries` is still available to the page.

Now, add a sidebar in the layout for the post page:

```svelte
/// file: src/routes/blog/[slug]/+layout.svelte
<script>
	let { data, children } = $props();
</script>

<div class="layout">
	<main>
		{@render children()}
	</main>

+++	<aside>
		<h2>More posts</h2>
		<ul>
			{#each data.summaries as { slug, title }}
				<li>
					<a href="/blog/{slug}">{title}</a>
				</li>
			{/each}
		</ul>
	</aside>+++
</div>

<style>
	@media (min-width: 640px) {
		.layout {
			display: grid;
			gap: 2em;
			grid-template-columns: 1fr 16em;
		}
	}
</style>
```

The layout (and any page below it) inherits `data.summaries` from the parent `+layout.server.js`.

When we navigate from one post to another, we only need to load the data for the post itself — the layout data is still valid. See the documentation on [invalidation](/docs/kit/load#Rerunning-load-functions) to learn more.
