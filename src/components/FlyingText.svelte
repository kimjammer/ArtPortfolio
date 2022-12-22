<script>
	import {onMount} from "svelte";

	export let scrollPercentage;
	let textElems = [];
	let pageReady = false;

	const text = ["Pixel Art", "Photography", "Doodles"];
	const startPosX = [100, 108, 120];
	const startPosY = [5, 90, 85];
	const scrollSpeeds = [2.5, 1.6, 1.8];

	//When the scrollPercentage changes, animate the name
	$: if (pageReady && scrollPercentage !== '') {
		textElems.forEach((elem, i) => {
			elem.animate({
				transform: `translate(${startPosX[i] + scrollPercentage * scrollSpeeds[i]}vw, ${startPosY[i]}vh)`
			}, {duration: 1000, fill: "forwards"});
		});
	}

	onMount(() => {
		pageReady = true;

		textElems.forEach((elem, i) => {
			elem.animate({
				transform: `translate(${startPosX[i]}vw, ${startPosY[i]}vh)`
			}, {duration: 0, fill: "forwards"});
		});
	})
</script>

<div class="wrapper">
	<!--For each text, create a div for it-->
	{#each text as text, i}
		<div class="text" bind:this={textElems[i]} style="transform: translate({startPosX[i]}, {startPosY[i]});">
			{text}
		</div>
	{/each}
</div>

<style>
    .wrapper {
        position:absolute;
        width: 100%;
        height: 100%;
        z-index: 5;
        display: flex;
		overflow: hidden;
    }
    .text{
        font-family: 'Rubik Light', sans-serif;
        font-size: 3em;
        color: #9395d3;
		user-select: none;
        filter: blur(3px);
        -webkit-filter: blur(2px);
    }
</style>