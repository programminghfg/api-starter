<script>
	export let movie;
	// console.log(movie);
</script>

{#if movie}
	<img src={'https://image.tmdb.org/t/p/w500' + movie.backdrop_path} />

	<div class="heading">
		<h1>{movie.title}</h1>
		<h2>
			<em>{movie.tagline}</em>
		</h2>
		<p>{movie.overview}</p>
		<a href={movie.homepage}>Movie Homepage</a>
	</div>
{:else}
	<div class="placeholder" />
	<div class="heading">
		<h1>Loading ...</h1>
	</div>
{/if}

<style>
	img {
		overflow: hidden;
		aspect-ratio: 3/2;
		object-fit: cover;
		width: 100%;
		border-radius: 1rem;
	}

	.placeholder {
		overflow: hidden;
		aspect-ratio: 3/2;
		object-fit: cover;
		width: 100%;
		background-color: gray;
		border-radius: 1rem;
	}

	h1 {
		font-size: 2rem;
	}

	h2 {
		font-size: 1.5rem;
		margin-bottom: 2rem;
	}

	p {
		columns: 2;
	}
</style>
