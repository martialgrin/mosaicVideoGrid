<script>
	export let name;
</script>

<div class="container">
	<div class="inner-container">
		<p>{name}</p>
	</div>
</div>

<style>
	.container {
		justify-self: start;
		align-self: flex-end;
		position: absolute;
	}
	.inner-container {
		position: flex;
	}

	p {
		font-family: "UnicaBold";
		font-size: 0.6rem;
		color: white;
		z-index: 10001;
	}
</style>
