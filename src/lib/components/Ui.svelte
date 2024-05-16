<script>
	export let _callback;
	function changeNumColumns() {
		const numColumns = document.getElementById("numColumns").value;
		_callback(numColumns);
		console.log(numColumns);
	}
</script>

<div class="ui-container">
	<p>N° Columns</p>
	<input
		on:input={changeNumColumns}
		type="range"
		id="numColumns"
		min="1"
		max="10"
		on:change={changeNumColumns}
	/>
</div>

<style>
	.ui-container {
		z-index: 1;
		position: fixed;
		top: -1rem;
		left: 0.5rem;
		padding: 1rem;
		background-color: white;
		display: flex;
		border-radius: 1rem;
	}

	p {
		font-size: 0.5rem;
		padding: 0.5rem;
	}
</style>
