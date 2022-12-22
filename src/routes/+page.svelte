<script>
	import Header from '../components/Header.svelte';
	import Carousel from "../components/Carousel.svelte";
	import FlyingText from "../components/FlyingText.svelte";
	import Popup from "../components/Popup.svelte";

	let carouselScroll;
</script>
<svelte:head>
	<title>John Kim</title>

	<link rel="preconnect" href="https://fonts.googleapis.com">
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
	<link href="https://fonts.googleapis.com/css2?family=Rubik:ital,wght@0,300;0,400;0,700;0,800;1,400&display=swap" rel="stylesheet">
</svelte:head>

<Popup/>
<Header bind:scrollPercentage={carouselScroll}/>
<Carousel bind:nextPercentage={carouselScroll}/>
<FlyingText bind:scrollPercentage={carouselScroll}/>

<style>
	/*Remove global body padding and margin*/
    :global(body) {
		margin: 0;
		padding: 0;
	}
</style>