<script>
	import { onMount } from 'svelte';
	import { page } from '$app/stores';

	import MovieDetailCard from '$lib/components/MovieDetailCard.svelte';
	let movieData;

	const href = $page.url;
	const segments = new URL(href).pathname.split('/');
	const movie_id = segments.pop() || segments.pop(); // Handle potential trailing slash
	// console.log(movie_id);

	onMount(async function () {
		const response = await fetch(
			` https://api.themoviedb.org/3/movie/${movie_id}?api_key=57bb4cbfa3753903764043c4efccd9dc&language=en-US`
		);

		const data = await response.json();
		// console.log(data);
		movieData = data;
	});
</script>

<a href="/">Home</a>
{#if movieData}
	<MovieDetailCard movie={movieData} />
{/if}
