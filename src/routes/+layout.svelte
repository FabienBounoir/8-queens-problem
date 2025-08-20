<script lang="ts">
	import '../app.scss';

	let { children } = $props();
</script>

<svelte:head>
	<title>8 Queens Problem - Interactive Solver | Chess Puzzle Game</title>

	<!-- Structured Data -->
	<script type="application/ld+json">
		{
			"@context": "https://schema.org",
			"@type": "WebApplication",
			"name": "8 Queens Problem Solver",
			"description": "Solve the 8 Queens Problem interactively. Visualize solutions, explore conflicts, and learn the logic behind this classic chess puzzle with our interactive solver.",
			"url": "https://8queensProblem.vercel.app/",
			"applicationCategory": "Game",
			"operatingSystem": "Web Browser",
			"author": {
				"@type": "Person",
				"name": "Fabien Bounoir"
			},
			"offers": {
				"@type": "Offer",
				"price": "0",
				"priceCurrency": "USD"
			}
		}
	</script>
</svelte:head>

{@render children?.()}
