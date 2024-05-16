<script>
	import { fade } from "svelte/transition";

	export let name, qrCode, projectName;
</script>

<div class="container" in:fade={{ duration: 500 }}>
	<div class="inner-container">
		<!-- <img src="./{qrCode}" alt={name} /> -->
		<p>{projectName}</p>
		<p class="name">{name}</p>
	</div>
</div>

<style>
	.container {
		backdrop-filter: blur(100px);
		justify-self: start;
		align-self: center;
		/* background-color: white; */
		position: absolute;
		display: flex;
		justify-content: center;
		align-items: center;
		width: 100vw;
		height: 100vh;
	}
	.inner-container {
		display: flex;
		flex-direction: column;
		justify-content: center;

		align-items: center;
		padding: 3rem;
		padding-bottom: 5rem;
		min-width: 30vw;
		min-height: 50vh;
	}
	p {
		text-align: center;
		font-weight: bold;
		/* font-family: Helvetica, Arial, sans-serif; */
		font-size: 7rem;
		padding-bottom: 2rem;
		color: white;
		z-index: 10001;
	}
	.name {
		font-size: 3.5rem;
	}
</style>
