<script>
	import { onMount } from "svelte";
	import Description from "./Description.svelte";

	export let project, row, otherRow, numProjects;
	let cardElem, videoElem;

	let isPlaying = false;

	const keyPressHandler = (e) => {};
	const clickHandler = (e) => {
		cardElem.style.flexGrow = !isPlaying ? 10 : 0;
		const otherElems = document.querySelectorAll(".card-video");
		if (!isPlaying) {
			videoElem.pause();
			setTimeout(() => {
				if (project.mode == "vertical") {
					videoElem.style.transform = isPlaying
						? "rotate(0deg)"
						: "rotate(90deg)";
				}
				setTimeout(
					() => {
						videoElem.currentTime = 0;
						videoElem.muted = !isPlaying ? false : true;
						videoElem.play();
						isPlaying = !isPlaying;
					},
					project.mode == "vertical" ? 2000 : 1000
				);
				videoElem.style.objectFit = !isPlaying ? "contain" : "cover";
			}, 2000);
		} else {
			if (project.mode == "vertical") {
				videoElem.style.transform = isPlaying
					? "rotate(0deg)"
					: "rotate(90deg)";
			}
			videoElem.muted = !isPlaying ? false : true;
			videoElem.play();
			setTimeout(() => {
				videoElem.style.objectFit = !isPlaying ? "contain" : "cover";
				isPlaying = false;
			}, 3000);
		}

		for (let i in otherElems) {
			if (otherElems[i] != cardElem) {
				if (typeof otherElems[i] == "object") {
					otherElems[i].style.width = !isPlaying
						? "0px"
						: " calc(100vw / " + numProjects / 2 + ")";
				}
			} else {
				cardElem.style.alignItems = !isPlaying ? "stretch" : "center";
				if (project.mode == "vertical") {
					otherElems[i].style.height = !isPlaying ? "100vh" : "50vh";
					const rapport = window.innerWidth / window.innerHeight;
					otherElems[i].style.transform = !isPlaying
						? "scale(" + rapport + ")"
						: "scale(1)";
				} else {
					otherElems[i].style.height = !isPlaying ? "100vh" : "50vh";
				}
				otherRow.style.height = !isPlaying ? "0vh" : "50vh";
				row.style.height = !isPlaying ? "100vh" : "50vh";
			}
		}
	};
</script>

<div
	bind:this={cardElem}
	style="--numProjects:{numProjects};"
	class="card-video"
	on:click={clickHandler}
	on:keyup={keyPressHandler}
>
	<video
		bind:this={videoElem}
		class="horizontal"
		src="./videos/{project.srcVideo}"
		loop
		autoplay
		muted
		playsinline
	/>

	<!-- <Description name={project.name} {isPlaying} /> -->
</div>

<style>
	.card-video {
		transition: all 1s ease;
		transition-delay: 1s;
		width: calc(100vw / calc(var(--numProjects) / 2));
		height: 50vh;
		object-fit: cover;
		overflow: hidden;
		background-size: cover;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	video {
		transition: transform 1s ease;
		min-width: 100%;
		min-height: 100%;
		object-fit: cover;
	}
</style>
