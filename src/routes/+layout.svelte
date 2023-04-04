<script>
	import { IconMenu2 } from '@tabler/icons-svelte'
	import { fly } from 'svelte/transition';
	import Sidebar from '../components/nav/Sidebar.svelte'

	let sidebar_show = false;

</script>

<style>
	main {
		/* background-color: blue; */
		height: 90%;
	}

	.wrapper {
		margin: 0;
		overflow-x: hidden;
		/* background-color: orangered; */
		min-width: 100vw;
		height: 100vh;
	}

	.menu-button{
		background: none;
		border: none;
	}

	header{
		display: flex;
		justify-content: space-between;
		height: 5%;
		touch-action: none;
		-ms-touch-action: none;
		z-index: 99;
	}

	footer{
		height: 5%;
	}

</style>
<div class="wrapper">
	<header>
		<a href="/">bgtools</a>
		<button class="menu-button" on:click={() => {sidebar_show = !sidebar_show}}>
			<IconMenu2 />
		</button>
	</header>

		<main>
			<slot />
			<Sidebar bind:show={sidebar_show} />
		</main>

	<footer>
		by compass
	</footer>
</div>

