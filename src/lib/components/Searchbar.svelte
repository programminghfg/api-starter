<script>
	import MovieCard from '$lib/components/MovieCard.svelte';

	// Input vom Input Field wird hier gespeichert
	let input = '';

	// Damit nicht sofort bei jeder Eingabe eine Anfrage gestellt wird, wird das Ganze durch eine Debounce-Function ausgebremst.
	// Debounce Input wird hier gespeichert
	let debouncedInput = '';
	// Der Debounce timer, der bei jeder Eingabe resetted wird.
	let timer;
	// Filme werden hier zwischengespeichert
	let movies = [];

	/**
	 * Debounce Function
	 * Bremst die Anfragen an den API-Server aus.
	 * Sobald die Function ausgelöst wird, wartet sie 500 Millisekunden, bis der Inhalt innendrin ausgeführt wird. Kommt eine Änderung in der Zwischenzeit, wird der Timer resetted und es wird wieder 500ms gewartet. (Spart API Anfragen!)
	 * @param inputVariable
	 */
	const debounce = (inputVariable) => {
		clearTimeout(timer);
		timer = setTimeout(() => {
			debouncedInput = inputVariable;

			// Falls der Input keinen Inhalt hat, ist die Stringlänge === 0. Dann soll nichts passieren. Wird ein Input mitgeliefert (input.length > 0), dann soll die Anfrage ausgeführt werden.
			if (debouncedInput.length > 0) {
				// Movies abrufen und in der movies-Variablen speichern.
				movies = getMovieData(debouncedInput);
				console.log(movies);
			}
			// Erst nach 500ms ausführen (kann beliebig geändert werden).
		}, 500);
	};

	/**
	 * Asynchrone Function, die Daten vom API Server abruft.
	 * Es kann der Parameter @param input mitgegeben werden. Der verändert in diesem Fall den Anfrage-String.
	 * @param input
	 */
	const getMovieData = async function (input) {
		// await deutet auf die asynchrone Ausführung hin. Wir warten auf die Daten des Servers und wollen die Gesamtapplikation damit nicht blockieren.
		const response = await fetch(
			// Template Strings -> Die Anführungsstriche sind hier bewusst in der Art gesetzt, da nur mit denen die Template String Syntax funktioniert. Wieso? Weil man dann bisschen eleganter in Strings (Zeichenketten) Variablen einfügen kann.
			// Syntax: `Stringinhalt blabla ${VARIABLEHIER}`
			`https://api.themoviedb.org/3/search/movie?api_key=57bb4cbfa3753903764043c4efccd9dc&page=1&query=${input}`
		);
		const data = await response.json();
		// console.log(data);

		// Daten mittels return zurückgeben. Damit kann die Rückgabe einer Funktion direkt in einer Variable gespeichert werden.
		return data.results;
	};
</script>

<div class="wrapper">
	<div class="searchgroup">
		<!-- 
			bind:value verwenden wir, um jeden Input direkt in der Variable "input" zu speichern. 
			on:keyup ist ein Event und wird ausgelöst, sobald die Taste losgelassen wird. Sobald der Event ausgelöst wird, wird die Funktion debounce aufgerufen.
		-->
		<input
			bind:value={input}
			class="search"
			type="text"
			id="search"
			placeholder="Quick search…"
			on:keyup={debounce(input)}
		/>
		<input class="submit" type="submit" value="Search" />
	</div>

	<p>{debouncedInput}</p>

	<div class="grid">
		<!-- Falls noch keine movies im Array gespeichert sind, wird der Fehler hier abgefangen. -->
		{#if movies.length === 0}
			{#each [...Array(20).keys()] as _}
				<MovieCard movie={null} />
			{/each}
		{:else}
			<!-- Ansonsten wird durch die Movies gelooped. -->
			{#await movies then result}
				{#each result as movieentry}
					<MovieCard movie={movieentry} />
				{/each}
			{/await}
		{/if}
	</div>
</div>

<style>
	/* Flex Layout Algorithmus: 1-dimensional, CSS Grid ist 2-dimensional. */
	.wrapper {
		display: flex;
		flex-direction: column;
		width: 100%;
	}
	input {
		box-sizing: border-box;
		font-size: 13px;
		vertical-align: top;
	}

	.searchgroup {
		display: inline-flex;
	}

	.search {
		display: flex;
		align-items: center;
		text-align: left;
		padding: 0 16px;
		background: rgb(47, 48, 48);
		height: 48px;
		width: 288px;
		border-radius: 8px;
		border: none;
		font-family: Inter, Helvetica, sans-serif;
		font-size: 18px;
		color: rgb(234, 234, 234);
		margin-right: 16px;
	}
	.search:focus {
		outline: none;
	}

	.submit {
		display: flex;
		align-items: center;
		text-align: left;
		padding: 0 16px;
		background: rgb(14, 165, 233);
		height: 48px;
		border-radius: 8px;
		border: none;
		font-family: Inter, Helvetica, sans-serif;
		font-size: 18px;
		color: rgb(255, 255, 255);
		cursor: pointer;
	}

	.grid {
		display: flex;
		grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
		grid-column-gap: 1rem;
		grid-row-gap: 2rem;

		grid-row-gap: 24px;
		grid-column-gap: 191px;
		grid-template-columns: repeat(3, calc(33.33333% - 32px));
	}
</style>
