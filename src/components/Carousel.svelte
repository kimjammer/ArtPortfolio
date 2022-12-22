<script>
	import {onMount} from "svelte";
	import ProjectPage from "./ProjectPage.svelte";
	import Project1 from "./Projects/Project1.svelte";
	import Project2 from "./Projects/Project2.svelte";
	import Project3 from "./Projects/Project3.svelte";
	import Project4 from "./Projects/Project4.svelte";
	import Project5 from "./Projects/Project5.svelte";
	import Project6 from "./Projects/Project6.svelte";
	import Project7 from "./Projects/Project7.svelte";
	import Project8 from "./Projects/Project8.svelte";

	let carouselEnabled = true;
	let mouseDownAt = 0;
	let percentage = 0;
	export let nextPercentage = 0;
	let prevPercentage = 0;
	const normalWidth = 40;
	const gapWidth = 4;

	const loadingScreenDuration = 200;
	let hideLoading = false;

	let loadingScreen;
	let track;

	let firstPageLocation;
	let lastPageLocation;

	const mousedown = e => {
		//If it is a touch event, get the first touch
		if (e.type === "touchstart") {
			e = e.touches[0];
		}

		mouseDownAt = e.clientX;
	}

	const mousemove = e => {
		if (mouseDownAt === 0 || !carouselEnabled) return;

		//If it is a touch event, get the first touch
		if (e.type === "touchmove") {
			e = e.touches[0];
		}

		const mouseDelta = mouseDownAt - e.clientX;
		const maxDelta = window.innerWidth / 1.5;

		percentage = (mouseDelta / maxDelta) * -100;
		nextPercentage = prevPercentage + percentage;
		nextPercentage = Math.min(0, Math.max(-100, nextPercentage));

		//Normalize the percentage to the firstPageLocation and lastPageLocation
		let normPercentage = ((nextPercentage + 100)/(0 + 100)) * (firstPageLocation - lastPageLocation) + lastPageLocation;

		track.animate({
			transform: `translate(${normPercentage}%, -50%)`
		}, {duration: 1200, fill: "forwards"});
		Array.from(track.children).forEach(child => {
			//Select the image element and animate.
			child.children[0].children[0].animate({
				objectPosition: `${normPercentage + 100}% center`
			}, {duration: 1200, fill: "forwards"});
		});
	}

	const mouseup = () => {
		mouseDownAt = 0;
		prevPercentage = nextPercentage;
	}

	//Scrolls the carousel so that the selected project is centered.
	const scrollToPage = (index, mode = null) => {
		//Toggles the carousel
		if (carouselEnabled && mode === null){
			carouselEnabled = false;
			mode ??= "expand";
		} else {
			carouselEnabled = true;
			mode ??= "shrink";
		}

		//Calculates the width of the expanded project page in vmin.
		let bigElemWidth;
		if (window.innerWidth > window.innerHeight){
			bigElemWidth = 100 * (window.innerWidth/window.innerHeight);
		} else {
			bigElemWidth = 100;
		}

		//Calculates the percentage of the carousel that needs to be scrolled.
		let totalWidth;
		let targetX;
		//If mode is "expand", use big elemWidth, if not, use normalWidth
		if (mode === "expand") {
			totalWidth = (track.children.length - 1) * (normalWidth + gapWidth) + bigElemWidth;
			targetX = index * (normalWidth + gapWidth) + bigElemWidth / 2;
		}else {
			totalWidth = (track.children.length - 1) * (normalWidth + gapWidth) + normalWidth;
			targetX = index * (normalWidth + gapWidth) + normalWidth / 2;
		}
		//This will map output the offset that the carousel needs to be scrolled to.
		let normPercentage = targetX / totalWidth * -100;
		//Un-Normalize the percentage so that it maps from 0 to -100
		nextPercentage = (normPercentage - lastPageLocation) / (firstPageLocation - lastPageLocation) * 100 - 100;
		prevPercentage = nextPercentage;

		//Animate the Carousel into place
		track.animate({
			transform: `translate(${normPercentage}%, -50%)`
		}, {duration: 1000, fill: "forwards", easing: "ease-in-out"});
		Array.from(track.children).forEach(child => {
			//Select the image element and animate.
			child.children[0].children[0].animate({
				objectPosition: `${normPercentage + 100}% center`
			}, {duration: 1000, fill: "forwards", easing: "ease-in-out"});
		});
	}

	onMount(() => {
		firstPageLocation = (normalWidth / 2) / ((track.children.length - 1) * (normalWidth + gapWidth) + normalWidth) * -100;
		lastPageLocation = ((track.children.length -1 ) * (normalWidth + gapWidth) + normalWidth / 2) / ((track.children.length - 1) * (normalWidth + gapWidth) + normalWidth) * -100;

		loadingScreen.animate({
			opacity: `0%`
		}, {duration: loadingScreenDuration, fill: "forwards", easing: "ease-in-out"})

		//Wait for the animation to finish
		setTimeout(() => {
			hideLoading = true;
		}, loadingScreenDuration);

		//Animate the Carousel into place
		track.animate({
			transform: `translate(${-100}%, -50%)`
		}, {duration: 0, fill: "forwards", easing: "ease-in-out"});
		Array.from(track.children).forEach(child => {
			//Select the image element and animate.
			child.children[0].children[0].animate({
				objectPosition: `${-100 + 100}% center`
			}, {duration: 0, fill: "forwards", easing: "ease-in-out"});
		});

		scrollToPage(0, "shrink");
	})
</script>
<svelte:window
		on:mousedown={mousedown}
		on:mousemove={mousemove}
		on:mouseup={mouseup}
		on:touchstart={mousedown}
		on:touchmove={mousemove}
		on:touchend={mouseup}
/>

<div class="wrapper" on:mousedown={mousedown} style="--imageOffset: {nextPercentage + 100}%;">
	<div id="image-track" bind:this={track}>
		<ProjectPage on:click={()=>{scrollToPage(0)}}>
			<img class="image" src="/images/1_Book.png" draggable="false" alt=""/>
			<span slot="content"><Project1/></span>
		</ProjectPage>
		<ProjectPage on:click={()=>{scrollToPage(1)}}>
			<img class="image" src="/images/2_OmoriSpace.png" draggable="false" alt=""/>
			<span slot="content"><Project2/></span>
		</ProjectPage>
		<ProjectPage on:click={()=>{scrollToPage(2)}}>
			<img class="image" src="/images/3_Key.png" draggable="false" alt=""/>
			<span slot="content"><Project3/></span>
		</ProjectPage>
		<ProjectPage on:click={()=>{scrollToPage(3)}}>
			<img class="image" src="/images/4_Room.png" draggable="false" alt=""/>
			<span slot="content"><Project4/></span>
		</ProjectPage>
		<ProjectPage on:click={()=>{scrollToPage(4)}}>
			<img class="image" src="/images/5_Hector.png" draggable="false" alt=""/>
			<span slot="content"><Project5/></span>
		</ProjectPage>
		<ProjectPage on:click={()=>{scrollToPage(5)}}>
			<img class="image" src="/images/6_BookOpen.png" draggable="false" alt=""/>
			<span slot="content"><Project6/></span>
		</ProjectPage>
		<ProjectPage on:click={()=>{scrollToPage(6)}}>
			<img class="image" src="/images/7_Gradients.png" draggable="false" alt=""/>
			<span slot="content"><Project7/></span>
		</ProjectPage>
		<ProjectPage on:click={()=>{scrollToPage(7)}}>
			<img class="image" src="/images/8_Pots.png" draggable="false" alt=""/>
			<span slot="content"><Project8/></span>
		</ProjectPage>
	</div>
</div>

<!--A loading screen div that will hide itself once the page is done loading-->
<div class="loading-screen" bind:this={loadingScreen} class:hideLoading>
	<div class="loading-screen-text">
		Loading...
	</div>
</div>

<style>
	.wrapper {
		position: absolute;
		width: 100%;
		height: 100%;
		background-color: #1f2041;
		margin: 0;
		padding: 0;
		overflow: hidden;
	}

	.image {
		border-radius: 8px;
		width: 40vmin;
		height: 56vmin;
		object-fit: cover;
		object-position: 100% center;
	}

	#image-track {
		display: flex;
		gap: 4vmin;
		position: absolute;
		left: 50%;
		top: 50%;
		transform: translate(0%, -50%);
		align-items: center;
		z-index: 100;
	}

	.loading-screen {
		position: absolute;
		width:100%;
		height:100%;
		background-color: #023618;
		z-index: 10000;
	}

	.loading-screen-text {
		position: absolute;
		left: 50%;
		top: 50%;
		transform: translate(-50%, -50%);
		font-size: 3rem;
		color: #77af9c;
		font-family: 'Rubik', sans-serif;
	}

	.hideLoading {
		display: none;
	}
</style>