<script>
	import dummydata from '$lib/api/dummydata';
	// console.log(dummydata);
	import MovieCard from '$lib/components/MovieCard.svelte';
</script>

<h1>Popular Movies</h1>
<div class="grid">
	{#each dummydata as dummyentry}
		<MovieCard movie={dummyentry} />
	{/each}
</div>

<style>
	.grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
		grid-column-gap: 1rem;
		grid-row-gap: 2rem;
	}
</style>
