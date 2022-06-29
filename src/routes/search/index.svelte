<script>
	import Searchbar from '$lib/components/Searchbar.svelte';
</script>

<div class="main">
	<Searchbar />
</div>

<style>
	.main {
		width: 80%;
		margin-left: auto;
		margin-right: auto;
		display: grid;
		grid-template-columns: repeat(3, minmax(300px, 1fr));
		grid-gap: 1rem;
	}
</style>
