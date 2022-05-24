<script>
	export let movie;
</script>

<article>
	<h1>{movie.title}</h1>
	<em>{movie.tagline}</em>
	<img src={'https://image.tmdb.org/t/p/w500' + movie.poster_path} />

	<p>{movie.overview}</p>
	<a href={movie.homepage}>Movie Homepage</a>
</article>
