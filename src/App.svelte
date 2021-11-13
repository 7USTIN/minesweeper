<script lang="ts">
	import Cell from "./components/Cell.svelte";
	import Header from "./components/Header.svelte";

	type cell = {
		isHidden: boolean;
		bomb: boolean;
		flag: boolean;
		neighbours: number;
	};

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

	function generateGrid(): cell[][] {
		let difficulty = difficulties[selectedDiff];
		let grid = [];

		for (let i = 0; i < difficulty.gridSize; i++) {
			grid.push([]);

			for (let j = 0; j < difficulty.gridSize; j++) {
				grid[i].push({
					isHidden: true,
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
					isHidden: true,
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

	function revealCells(rowIdx: number, cellIdx: number) {
		grid[rowIdx][cellIdx].isHidden = false;

		if (grid[rowIdx][cellIdx].bomb || grid[rowIdx][cellIdx].neighbours) {
			return;
		}

		for (let i = -1; i < 2; i++) {
			for (let j = -1; j < 2; j++) {
				if (!grid[rowIdx + i]?.[cellIdx + j]) {
					break;
				}

				if (
					!grid[rowIdx + i][cellIdx + j].bomb &&
					grid[rowIdx + i][cellIdx + j].isHidden
				) {
					revealCells(rowIdx + i, cellIdx + j);
				}
			}
		}
	}

	function reset() {
		grid = generateGrid();
		game = { time: 0, flags: 0 };
	}
</script>

<main>
	<Header bind:selectedDiff {game} {difficulties} on:diffChange={reset} />

	<section
		style={`grid-template-columns: repeat(${difficulty.gridSize}, 1fr);`}
	>
		{#each grid as row, rowIdx (rowIdx)}
			{#each row as { bomb, flag, neighbours, isHidden }, cellIdx (cellIdx)}
				<Cell
					on:reveal={() => revealCells(rowIdx, cellIdx)}
					bind:flag
					bind:game
					bind:isHidden
					{difficulty}
					{bomb}
					{neighbours}
				/>
			{/each}
		{/each}
	</section>
</main>

<style lang="scss">
	:global(:root) {
		--black: hsl(197, 8%, 8%);
		--white: hsl(241, 3%, 93%);
		--white-dark: hsl(241, 3%, 83%);
		--gray: hsl(208, 7%, 37%);
		--gray-dark: hsl(197, 6%, 15%);
	}

	:global(*) {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
		outline: none;
	}

	:global(body, button, input) {
		font-family: "Inter", sans-serif;
		font-size: 14px;
		background: var(--black);
		color: var(--white);
	}

	:global(body) {
		width: 100vw;
		height: 100vh;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}

	main {
		display: flex;
		flex-direction: column;
	}

	section {
		user-select: none;
		-moz-user-select: none;
		-webkit-user-select: none;
		display: grid;
	}
</style>
