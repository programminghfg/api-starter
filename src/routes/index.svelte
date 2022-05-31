<script>
	import { onMount } from 'svelte';
	import MovieCard from '$lib/components/MovieCard.svelte';

	const getMovieData = async function () {
		const response = await fetch(
			`https://api.themoviedb.org/3/discover/movie?api_key=57bb4cbfa3753903764043c4efccd9dc&language=en-US&sort_by=popularity.desc&include_adult=false&include_video=false&page=1&primary_release_date.gte=2022-05-01`
		);

		const data = await response.json();
		// console.log(data);
		movies = await data.results;
	};

	let movies = getMovieData();

	onMount(async function () {
		console.log('hello');
		movies = getMovieData();
	});
</script>

<div>
	<h1>Movies in theater right now</h1>
	<div class="grid">
		{#await movies}
			{#each [...Array(20).keys()] as _}
				<MovieCard movie={null} />
			{/each}
		{:then movies}
			{#each movies as movieentry}
				<MovieCard movie={movieentry} />
			{/each}
		{/await}
	</div>
</div>

<style>
	.grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
		grid-column-gap: 1rem;
		grid-row-gap: 2rem;
	}
</style>
