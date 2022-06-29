<script>
	import IconButton from './IconButton.svelte';
	import IconTextButton from './IconTextButton.svelte';
</script>

<header>
	<nav>
		<img class="logo" src="Logo.svg" alt="Logo" />
		<ul>
			<IconButton />
			<IconTextButton />
			<li>Home</li>
			<li>Impressum</li>
			<li>Profil</li>
		</ul>
	</nav>
</header>

<style>
	nav {
		width: 100%;
		display: inline-block;
	}

	.logo {
		float: left;
	}

	/* Navigation ul css */
	ul {
		float: right;
		list-style-type: none;
		display: flex;
	}

	ul li {
		display: inline-block;
		padding: 10px;
		margin: 0;
		/* list-style: none; */
	}
</style>
