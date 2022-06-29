<script>
	import { onMount } from 'svelte';
	// Component import
	import MovieCard from '$lib/components/MovieCard.svelte';

	/**
	 * Funktion um Filme von der API zu laden
	 * Asynchron -> Wir haben keine Garantie, dass die Antwort vom Server sofort kommt. Wenn die Funktion nicht asynchron läuft, reagiert die Website währenddessen nicht und nimmt keine Eingaben beispielsweise an.
	 * Daher das Stichwort 'async', sodass der Browser diese Anfrage einfach im Hintergrund laufen lässt und sobald eine Antwort da ist, diese weiterverarbeitet.
	 * */
	const getMovieData = async function () {
		const response = await fetch(
			`https://api.themoviedb.org/3/discover/movie?api_key=57bb4cbfa3753903764043c4efccd9dc&language=en-US&sort_by=popularity.desc&include_adult=false&include_video=false&page=1&primary_release_date.gte=2022-05-01`
		);

		/**
		 * In der response ist die Anfrage gespeichert. Im Body der Antwort stehen unsere Daten.
		 * Um die lesen zu können, müssen sie von einem Stream in lesbares JSON (Javascript Object Notation) konvertiert werden. -> response.json()
		 */
		const data = await response.json();
		// console.log(data);

		// Aus dem data JSON-Object verwenden wir den Key results -> data.results
		movies = await data.results;
	};

	let movies = getMovieData();

	// Wird ausgeführt, sobald die DOM (= die Inhalte im Browser) dargestellt werden.
	onMount(async function () {
		console.log('hello');
		// Hier werden die Filme geladen (Funktionsaufruf und anschließend den zurückgegebenen Wert in der Variable movies speichern)
		movies = getMovieData();
	});
</script>

<div class="main">
	<h1>Movies in theater right now</h1>

	<div class="grid">
		<!-- Warten auf die asynchrone Befüllung der Variable -> await -->
		{#await movies}
			<!-- Solange keine Daten vorhanden sind mit Platzhalter Daten arbeiten -->
			{#each [...Array(20).keys()] as _}
				<MovieCard movie={null} />
			{/each}

			<!-- Jetzt sind die Daten da, also weiter gehts -->
		{:then movies}
			<!-- Durch alle Daten durchloopen -->
			{#each movies as movieentry}
				<!-- Jeden einzelnen Eintrag der Component übergeben -->
				<MovieCard movie={movieentry} />
			{/each}
		{/await}
	</div>
</div>

<style>
	.main {
		width: 80%;
		margin-left: auto;
		margin-right: auto;
	}

	/* 
		CSS Grids – Yay 
	  display:grid wählt den Grid-Layout-Algorithmus aus
	*/
	.grid {
		display: grid;
		/* Nicer shortcut für responsive Grid Layout */
		grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
		/* Spalten und Row Abstände */
		grid-column-gap: 1rem;
		grid-row-gap: 2rem;
	}
</style>
