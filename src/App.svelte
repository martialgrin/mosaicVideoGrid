<script>
	import svelteLogo from "./assets/svelte.svg";
	import Card from "./lib/components/Card.svelte";
	import Counter from "./lib/Counter.svelte";
	import { projectsArray } from "./lib/videosName";

	let firstRow, secondRow;
</script>

<!-- <header>
	<h1>THEY CAN DRAW, THEY CAN MOVE, BUT CAN THEY DANCE ?</h1>
</header> -->
<div id="app">
	<div class="row" bind:this={firstRow}>
		{#each projectsArray as project, i}
			{#if i < 3}
				<Card {project} row={firstRow} otherRow={secondRow} />
			{/if}
		{/each}
	</div>
	<div id="break" />
	<div class="row" bind:this={secondRow}>
		{#each projectsArray as project, i}
			{#if i >= 3}
				<Card {project} row={secondRow} otherRow={firstRow} />
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
		grid-template-columns: repeat(3, calc(100vw / 3));
		grid-template-rows: repeat(2, 50vh);
	}
</style>
