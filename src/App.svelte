<script>
	import svelteLogo from "./assets/svelte.svg";
	import Card from "./lib/components/Card.svelte";
	import Ui from "./lib/components/Ui.svelte";
	import Counter from "./lib/Counter.svelte";
	import { projectsArray } from "./lib/videosName";

	let firstRow, secondRow;

	const numProjectsLength = projectsArray.length;

	let numProjects = numProjectsLength; // pairedNumberPlease
	let numColumns = 4;
	let videosExtension = "mp4";
	let isOpen = null;
	let colOpen = null;
	let qrCodeMode = false;
	let pArray = [];
	let mutedMode = true;

	$: numColumns;
	$: numProjects;

	if (numProjects < numColumns) {
		numProjects = numColumns;
	}

	let numProjectsEven = numProjects % 2 === 0 ? numProjects : numProjects + 1;

	function onKeyDown(e) {
		switch (e.key) {
			case "9":
				qrCodeMode = !qrCodeMode;
				break;

			case "m":
				console.log("helloWolrdm");
				mutedMode != mutedMode;
			default:
				break;
		}
	}
	function clickHandler(index, col) {
		if (isOpen != null) {
			isOpen = null;
			colOpen = null;
		} else {
			colOpen = col;
			isOpen = index;
		}
	}

	function changeNumColumns(num) {
		numColumns = num;
		numProjects = numProjectsLength;
		if (numProjects < numColumns) {
			numProjects = numColumns;
		}
		numProjectsEven = numProjects % 2 === 0 ? numProjects : numProjects + 1;
	}
	function projectIsInColumn(i, col) {
		if (
			i >= Math.floor(numProjectsLength / numColumns) * col &&
			i < Math.floor(numProjectsLength / numColumns) * (col + 1) &&
			projectsArray.length > i
		) {
			return true;
		} else {
			return false;
		}
	}
</script>

<div id="app" style="--numColumns:{numColumns}">
	<!-- <Ui _callback={changeNumColumns} /> -->
	{#each { length: numColumns } as _, col}
		<div
			class="columns {col == colOpen && colOpen != null
				? 'open-mode'
				: colOpen == null
					? 'grid-mode'
					: 'hidden-mode'}"
		>
			{#each { length: numProjects } as _, i}
				{#if projectIsInColumn(i, col)}
					<Card
						bind:this={pArray[i]}
						on:click={() => {
							clickHandler(i, col);
						}}
						{col}
						{mutedMode}
						{qrCodeMode}
						index={i}
						isOpenIndex={isOpen}
						id={"project" + i}
						{numProjects}
						{colOpen}
						project={projectsArray[i]}
						row={firstRow}
						otherRow={secondRow}
						isOpen={isOpen == i ? true : false}
					/>
				{/if}
			{/each}
		</div>
	{/each}
</div>
9

<!-- <svelte:window on:keydown|preventDefault={onKeyDown} /> -->

<style>
	#app {
		height: 100vh;
		display: grid;
		grid-template-columns: 1fr;
	}
	header {
		position: fixed;
	}

	.open-mode {
		width: 100vw;
	}
	.columns.grid-mode {
		width: calc(100vw / var(--numColumns));
		transition-property: width;
		transition-duration: 1s;
		transition-delay: 2s;
	}
	.columns.hidden-mode {
		width: 0vw;
	}

	.columns {
		grid-row: 1;
		height: 100vh;
		transition-property: width;
		transition-duration: 2s;
		transition-delay: 0s;
		display: flex;
		flex-direction: column;
		justify-content: stretch;
		overflow: hidden;
	}
</style>
