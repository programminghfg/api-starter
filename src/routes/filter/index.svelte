<script>
	import MovieCard from '$lib/components/MovieCard.svelte';

	const getGenres = async function () {
		const response = await fetch(
			` https://api.themoviedb.org/3/genre/movie/list?api_key=57bb4cbfa3753903764043c4efccd9dc&language=en-US`
		);

		const data = await response.json();
		console.log(data);
		return await data.genres;
	};
	let genres = getGenres();

	let dropdownOpen = false;
	let selected = '';

	function handleClick() {
		dropdownOpen = !dropdownOpen;
	}

	function handleSelect(genre) {
		console.log(genre);
		selected = genre;
		dropdownOpen = false;
		movies = getMovieData(selected);
	}

	const getMovieData = async function (genre) {
		let apiCall = `https://api.themoviedb.org/3/discover/movie?api_key=57bb4cbfa3753903764043c4efccd9dc&language=en-US&sort_by=popularity.desc&include_adult=false&with_genres=${genre}`;
		console.log(apiCall);
		const response = await fetch(apiCall);

		const data = await response.json();
		movies = await data.results;
		console.log(data.results);
	};

	let movies = getMovieData();
</script>

<div class="wrapper">
	<div class="dropdown">
		<div>
			<button class="button" type="submit" value="Search" on:click={handleClick}>
				Genres
				<svg
					class="chevron-down"
					xmlns="http://www.w3.org/2000/svg"
					viewBox="0 0 20 20"
					fill="currentColor"
					aria-hidden="true"
				>
					<path
						fill-rule="evenodd"
						d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z"
						clip-rule="evenodd"
					/>
				</svg>
			</button>
		</div>

		<div class="menu {dropdownOpen ? 'menu-open' : ''}">
			{#await genres then genres}
				{#each genres as genre}
					<p class="menu-item" on:click={() => handleSelect(genre.id)}>{genre.name}</p>
				{/each}
			{/await}
		</div>
	</div>

	<div class="grid">
		{#await movies then movies}
			{#each movies as movie}
				<MovieCard {movie} />
			{/each}
		{/await}
	</div>
</div>

<style>
	.wrapper {
		display: flex;
		flex-direction: column;
		width: 100%;
	}

	.dropdown {
		position: relative;
		display: inline-block;
		text-align: left;
	}

	.dropdown .button {
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

	.chevron-down {
		width: 24px;
		height: 24px;
		margin-left: 12px;
		display: inline-block;
	}

	.dropdown .menu {
		position: absolute;
		display: none;
		flex-direction: column;
		gap: 6px;
		margin-top: 12px;
		border-radius: 8px;
		padding: 12px 0;
		background: white;
		box-shadow: rgb(255, 255, 255) 0px 0px 0px 0px, rgba(0, 0, 0, 0.05) 0px 0px 0px 1px,
			rgba(0, 0, 0, 0.1) 0px 10px 15px -3px, rgba(0, 0, 0, 0.1) 0px 4px 6px -4px;
	}

	.menu-open {
		display: flex !important;
	}

	.dropdown .menu .menu-item {
		font-family: Inter, Helvetica, sans-serif;
		cursor: pointer;
		padding: 12px;
	}

	.dropdown .menu .menu-item:hover {
		background: rgb(14, 165, 233);
	}

	.grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
		grid-column-gap: 1rem;
		grid-row-gap: 2rem;

		grid-row-gap: 24px;
		grid-column-gap: 191px;
		grid-template-columns: repeat(3, calc(33.33333% - 32px));
	}
</style>
