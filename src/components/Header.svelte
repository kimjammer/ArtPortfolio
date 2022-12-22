<script>
	import {onMount} from "svelte";

	export let scrollPercentage;
	let nameText;
	let pageReady = false;

	//When the scrollPercentage changes, animate the name
	$: if (scrollPercentage !== '' && pageReady) {
		nameText.animate({
			transform: `translateX(${scrollPercentage * 1.5}%)`
		}, {duration: 1000, fill: "forwards"});
	}

	onMount(() => {
		pageReady = true;
	})

</script>

<div class="wrapper">
	<h1 bind:this={nameText}>John Kim</h1>
</div>

<style>
	.wrapper {
		position:absolute;
		width: 100%;
		height: 20vh;
		z-index: 10;
		display: flex;
		justify-content: center;
		align-content: center;
	}
	h1{
		font-family: 'Rubik', sans-serif;
		font-size: 5em;
		color: #9395d3;
		user-select: none;
        text-shadow: #1f2041 0 0 10px;
	}
</style>