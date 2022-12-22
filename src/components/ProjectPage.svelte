<script>
	import {onMount} from "svelte";

	export let selected = false;

	let imageWrapper;
	let content;
	let image;

	onMount(()=>{
		image = imageWrapper.children[0];
	})

	const handleClick = () => {
		if (!selected){
			expand();
		}else {
			shrink();
		}
	}

	const expand = () => {
		selected = true;
		image.animate({
			width: `100vw`,
			height: `100vh`,
			borderRadius: `0px`
		}, {duration: 1200, easing: "ease-in-out", fill: "both"});
		content.animate({
			width: `100vw`,
			height: `100vh`,
			opacity: `100%`
		}, {duration: 1200, easing: "ease-in-out", fill: "both"});
	}

	const shrink = () => {
		selected = false;
		image.animate({
			width: `40vmin`,
			height: `56vmin`,
			borderRadius: `8px`,
			objectPosition: `center center`
		}, {duration: 1200, easing: "ease-in-out", fill: "both"});
		content.animate({
			width: `40vmin`,
			height: `56vmin`,
			opacity: `0%`
		}, {duration: 1200, easing: "ease-in-out", fill: "both"});
	}
</script>

<div class="wrapper" on:click={handleClick} on:keypress={handleClick} on:click>
	<span bind:this={imageWrapper}><slot></slot></span>
	<div bind:this={content} class="content"><slot name="content"></slot></div>
</div>

<style>
	.wrapper {
		user-select:none;
		display: flex;
	}
	.content {
		position: absolute;
		width: 40vmin;
		height: 56vmin;
		opacity: 0;
	}
</style>