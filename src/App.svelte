<script lang="ts">
	import Cell from "./components/Cell.svelte";
	import Header from "./components/Header.svelte";

	const difficulties = {
		Easy: {
			flags: 10,
			gridSize: 10,
		},
		Medium: {
			flags: 40,
			gridSize: 18,
		},
		Hard: {
			flags: 99,
			gridSize: 25,
		},
	};

	let selectedDiff = localStorage.getItem("selectedDiff") || "Medium";
	let grid = generateGrid();
	let game = {
		flags: 0,
		time: 0,
	};

	$: difficulty = difficulties[selectedDiff];

	function randomIndex(array: any[]): number {
		return (array.length * Math.random()) | 0;
	}

	function generateGrid(): [][] {
		let difficulty = difficulties[selectedDiff];
		let grid = [];

		for (let i = 0; i < difficulty.gridSize; i++) {
			grid.push([]);

			for (let j = 0; j < difficulty.gridSize; j++) {
				grid[i].push({
					bomb: false,
					flag: false,
					neighbours: 0,
				});
			}
		}

		for (let i = 0; i < difficulty.flags; i++) {
			let randomNums = [randomIndex(grid), randomIndex(grid)];

			if (grid[randomNums[0]][randomNums[1]].bomb) {
				i--;
			} else {
				grid[randomNums[0]][randomNums[1]] = {
					bomb: true,
					flag: false,
					neighbours: 0,
				};

				for (let i = -1; i < 2; i++) {
					for (let j = -1; j < 2; j++) {
						if ([-1, grid.length].includes(randomNums[0] + i)) {
							continue;
						}

						if (grid[randomNums[0] + i][randomNums[1] + j]) {
							grid[randomNums[0] + i][randomNums[1] + j]
								.neighbours++;
						}
					}
				}
			}
		}

		return grid;
	}

	function reset() {
		grid = generateGrid();
		game = { time: 0, flags: 0 };
	}
</script>

<main>
	<div class="wrapper">
		<Header bind:selectedDiff {game} {difficulties} on:diffChange={reset} />

		<section>
			<div
				style={`grid-template-columns: repeat(${difficulty.gridSize}, 1fr)`}
			>
				{#each grid as row, rowIdx (rowIdx)}
					{#each row as { bomb, flag, neighbours }, fieldIdx (fieldIdx)}
						<Cell
							bind:flag
							bind:game
							{difficulty}
							{bomb}
							{neighbours}
						/>
					{/each}
				{/each}
			</div>
		</section>
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

	section {
		user-select: none;
		-moz-user-select: none;
		-webkit-user-select: none;

		div {
			display: grid;
		}
	}
</style>
