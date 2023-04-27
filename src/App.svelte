<script>
	import svelteLogo from "./assets/svelte.svg";
	import Card from "./lib/components/Card.svelte";
	import Counter from "./lib/Counter.svelte";
	import { projectsArray } from "./lib/videosName";

	let firstRow, secondRow;

	let numProjects = 8; // pairedNumberPlease
</script>

<!-- <header>
	<h1>THEY CAN DRAW, THEY CAN MOVE, BUT CAN THEY DANCE ?</h1>
</header> -->
<div id="app">
	<div class="row" bind:this={firstRow} style="--numProjects:{numProjects};">
		{#each projectsArray as project, i}
			{#if i < numProjects / 2}
				<Card {numProjects} {project} row={firstRow} otherRow={secondRow} />
			{/if}
		{/each}
	</div>
	<div id="break" />
	<div class="row" bind:this={secondRow} style="--numProjects:{numProjects};">
		{#each projectsArray as project, i}
			{#if i >= numProjects / 2}
				<Card {numProjects} {project} row={secondRow} otherRow={firstRow} />
			{/if}
		{/each}
	</div>
</div>

<style>
	#app {
		height: 100vh;
	}
	header {
		position: fixed;
	}

	.row {
		transition: all 1s ease;
		display: flex;
		flex-wrap: wrap;

		width: 100vw;
		height: 50vh;
		overflow: hidden;
		grid-template-columns: repeat(
			calc(var(--numProjects) / 2),
			calc(100vw / calc(var(--numProjects) / 2))
		);
		grid-template-rows: repeat(2, 50vh);
	}
</style>
