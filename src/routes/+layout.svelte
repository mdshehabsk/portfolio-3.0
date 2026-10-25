<script lang="ts">
	import './layout.css';
	import favicon from '../images/fav.jpg'
	import type { LayoutProps } from './$types';

	let { children }: LayoutProps = $props();
</script>

<svelte:head>
    <title> Shehab Uddin - Software Engineer</title>
	<link rel="icon" href={favicon} />
</svelte:head>

{@render children()}
