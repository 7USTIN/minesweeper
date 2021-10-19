<script lang="ts">
	import Grid from './components/Grid.svelte';
	import Header from './components/Header.svelte';

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

	let selectedDiff = localStorage.getItem('selectedDiff') || 'Medium';
	let time = 0;
</script>

<main>
	<Header bind:selectedDiff {time} {difficulties} />
	<Grid bind:time {selectedDiff} {difficulties} />
</main>

<style lang="scss">
	:global(:root) {
		--bg: #ffffff;
		--second-bg: #f8f9fa;
		--text: #151515;
		--second-text: #59595a;
		--border: #e4e4e4;
	}

	:global([data-theme='dark']) {
		--bg: #151515;
		--second-bg: #1c1c1c;
		--text: #ffffff;
		--second-text: #adadad;
		--border: #333333;
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

	:global(body, button, input, option) {
		font-family: 'Inter', sans-serif;
		font-size: 14px;
		background: var(--bg);
		color: var(--text);
	}

	main {
		width: 100vw;
		height: 100vh;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}
</style>
