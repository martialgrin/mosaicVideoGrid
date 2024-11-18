<script>
	import { onMount } from "svelte";
	import Description from "./Description.svelte";
	import { createEventDispatcher } from "svelte";
	import { select_option } from "svelte/internal";

	const dispatch = createEventDispatcher();

	export let project,
		row,
		otherRow,
		colOpen,
		col,
		numProjects,
		id,
		isOpen,
		index,
		isOpenIndex,
		mutedMode,
		qrCodeMode;
	let cardElem, videoElem, imageElem;

	let timeOutFit = null;
	let timeOutPlay = null;
	let timeOutRemoveImage = null;
	let openDescription = false;
	let descriptionHasBeenOpened = false;

	const videoState = (state) => {
		console.log("videoState", state);

		if (typeof videoElem != "undefined" && videoElem != null) {
			if (state == true) {
				timeOutRemoveImage = setTimeout(() => {
					imageElem.style.display = "none";
				}, 4000);
				timeOutPlay = setTimeout(() => {
					videoElem.currentTime = 0;
					videoElem.muted = false;
					videoElem.play();
				}, 4000);
			} else {
				videoElem.muted = true;
				videoElem.style.objectFit = "cover";
			}
		}
	};

	const muteVideo = () => {
		console.log("mute");
		if (typeof videoElem != "undefined" && videoElem != null) {
			videoElem.muted = true;
		}
	};

	$: {
		if (isOpen == true) {
			descriptionHasBeenOpened = false;
			clearTimeout(timeOutFit);
			clearTimeout(timeOutPlay);
			clearTimeout(timeOutRemoveImage);

			muteVideo();
			videoState(true);
		} else {
			descriptionHasBeenOpened = false;
			clearTimeout(timeOutFit);
			clearTimeout(timeOutPlay);
			clearTimeout(timeOutRemoveImage);

			muteVideo();
			videoState(false);
		}
	}

	const videoIsFinished = () => {
		if (!descriptionHasBeenOpened && isOpen) {
			descriptionHasBeenOpened = true;
			openDescription = true;
			setTimeout(() => {
				relaunchMutedVideo();
			}, 5000);
		} else {
			relaunchMutedVideo();
		}
	};

	const relaunchMutedVideo = () => {
		videoElem.currentTime = 0;
		videoElem.play();
		videoElem.muted = true;
		openDescription = false;
	};
</script>

<div
	bind:this={cardElem}
	class={isOpenIndex != index && isOpenIndex != null && colOpen != col
		? "card-video hide-other-col"
		: isOpenIndex != index && isOpenIndex != null
			? "card-video hide"
			: isOpen
				? "card-video open"
				: "card-video"}
	{id}
	on:click
>
	<video
		bind:this={videoElem}
		class="horizontal"
		src="./videos/{project.srcVideo}.mp4"
		on:ended={() => {
			videoIsFinished();
		}}
		autoplay={false}
		muted
		playsinline
	/>

	{#if openDescription}
		<Description
			projectName={project.projectName}
			name={project.name}
			qrCode={project.qrCode}
		/>
	{/if}
	<img
		bind:this={imageElem}
		src="./pictures/{project.srcImage}"
		alt={project.projectName}
	/>
</div>

<style>
	.card-video.hide {
		width: 100vw !important;
		height: 0 !important;
		transition-property: height;
		transition-timing-function: ease-out;
		transition-duration: 1s;
		transition-delay: 2s;
	}
	.card-video.hide-other-col {
		width: 0 !important;
		height: 100 !important;
		transition-property: height, width;
		transition-timing-function: ease-out;
		transition-duration: 1s, 1s;
		transition-delay: 1.5s, 1s;
	}
	.card-video {
		transition-property: height, width;
		transition-timing-function: ease-out;
		transition-duration: 1s, 1s;
		transition-delay: 0s, 0s;
		width: 100%;
		height: 100%;
		min-width: 100%;
		object-fit: cover;
		overflow: hidden;
		background-size: cover;
		display: flex;
		justify-content: center;
		align-items: center;
		position: relative;
	}

	video {
		width: 100%;
		transition: all 1s ease;
		min-width: 33.33vw;
		min-height: 100%;
		object-fit: cover;
	}
	img {
		position: absolute;
		object-fit: cover;
		width: 100%;
		min-height: 100%;
	}
</style>
