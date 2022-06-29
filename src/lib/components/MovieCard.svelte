<script>
	import { fade } from 'svelte/transition';
	import { onMount } from 'svelte';
	import lottie from 'lottie-web';
	import lottieAnimation from '$lib/assets/97930-loading';
	export let movie;

	let lottieElement;
	onMount(async function () {
		lottie.loadAnimation({
			container: lottieElement, // the dom element that will contain the animation
			renderer: 'svg',
			loop: true,
			autoplay: true,
			animationData: lottieAnimation
		});
	});
</script>

{#if movie}
	<div class="movie-card" in:fade>
		<a href={'/movie/' + movie.id}
			><img src={'https://image.tmdb.org/t/p/w500' + movie.poster_path} alt={movie.title} /></a
		>
		<div class="description">
			<h2>{movie.title}</h2>
			<p>{movie.overview}</p>
			<em>{movie.release_date}</em>
		</div>
	</div>
{:else}
	<div class="movie-card" in:fade>
		<div bind:this={lottieElement} />
		<div class="description">
			<h2>Loading</h2>
		</div>
	</div>
{/if}

<style>
	img {
		width: 100%;
		aspect-ratio: 3/4;
		object-fit: cover;
		border-radius: 1rem;
	}

	.placeholder {
		width: 100%;
		aspect-ratio: 3/4;
		object-fit: cover;
		border-radius: 1rem;
		background: gray;
	}

	h2 {
		font-size: 0.9rem;
	}
	.description {
		margin-bottom: 1rem;
	}
	p {
		font-size: 0.7rem;
		text-overflow: ellipsis;
		overflow: hidden;
		display: -webkit-box !important;
		-webkit-line-clamp: 2;
		-webkit-box-orient: vertical;
		white-space: normal;
	}

	.movie-card {
		display: flex;
		flex-direction: column;
		justify-content: space-around;
		padding: 1rem;
	}
</style>
