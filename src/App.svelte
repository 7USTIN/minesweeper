<script lang="ts">
	import Grid from "./components/Grid.svelte";
	import Header from "./components/Header.svelte";

	const difficulties = {
		Easy: {
			flagCount: 10,
			gridSize: 10,
		},
		Medium: {
			flagCount: 40,
			gridSize: 18,
		},
		Hard: {
			flagCount: 99,
			gridSize: 25,
		},
	};

	let selectedDiff = localStorage.getItem("selectedDiff") || "Easy";
	let time = 0;
</script>

<main>
	<div class="wrapper">
		<Header bind:selectedDiff {time} {difficulties} />
		<Grid bind:time {selectedDiff} {difficulties} />
	</div>
</main>

<style lang="scss">
	:global(:root) {
		--black: hsl(197, 8%, 8%);
		--white: hsl(241, 3%, 93%);
		--white-dark: hsl(241, 3%, 83%);
		--gray: hsl(208, 7%, 37%);
		--gray-dark: hsl(197, 6%, 12%);
		--accent: hsl(193, 95%, 45%);
	}

	:global(*) {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
		outline: none;
	}

	:global(html) {
		scroll-behavior: smooth;
	}

	:global(body, button, input) {
		font-family: "Inter", sans-serif;
		font-size: 14px;
		background: var(--black);
		color: var(--white);
	}

	main {
		width: 100vw;
		height: 100vh;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;

		.wrapper {
			display: flex;
			flex-direction: column;
		}
	}
</style>
