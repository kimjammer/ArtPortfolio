<script>
	let showPopup = true;
	let wrapper;

	function dismissPopup() {
		wrapper.animate({
			opacity: `0%`
		}, {duration: 300, fill: "forwards", easing: "ease-in-out"})

		//Wait for the animation to finish
		setTimeout(() => {
			showPopup = false;
		}, 300);
	}
</script>
<svelte:window
		on:mousedown={dismissPopup}
		on:touchstart={dismissPopup}
/>

{#if showPopup}
	<div class="wrapper" bind:this={wrapper}>
		<div class="popup">
			Remember, I'm not an artist.
		</div>
	</div>

{/if}

<style>
    .popup {
        position: fixed;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        background-color: #9395d3;
        padding: 30px;
        border-radius: 5px;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
		z-index: 9999;
		font-family: Rubik, sans-serif;
    }
	.wrapper {
		position: fixed;
		width: 100%;
		height: 100%;
		backdrop-filter: blur(5px);
		z-index: 9998;
	}
</style>
