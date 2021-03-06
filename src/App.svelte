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
	let game = {
		flags: 0,
		time: 0,
		state: "IN_GAME",
		bombs: [],
		revealedNonBombs: 0,
	};
	let grid = generateGrid();

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
			let pos = {
				x: randomIndex(grid),
				y: randomIndex(grid),
			};

			if (grid[pos.y][pos.x].bomb) {
				i--;
			} else {
				grid[pos.y][pos.x] = {
					isHidden: true,
					bomb: true,
					flag: false,
					neighbours: 0,
				};

				game.bombs.push([pos.y, pos.x]);

				for (let i = -1; i < 2; i++) {
					for (let j = -1; j < 2; j++) {
						if (grid[pos.y + i]?.[pos.x + j]) {
							grid[pos.y + i][pos.x + j].neighbours++;
						}
					}
				}
			}
		}

		return grid;
	}

	function revealCells(y: number, x: number) {
		grid[y][x].isHidden = false;

		checkLose(y, x);

		if (!grid[y][x].neighbours) {
			for (let i = -1; i < 2; i++) {
				for (let j = -1; j < 2; j++) {
					if (
						grid[y + i]?.[x + j] &&
						!grid[y + i][x + j].bomb &&
						grid[y + i][x + j].isHidden
					) {
						revealCells(y + i, x + j);
					}
				}
			}
		}

		game.revealedNonBombs++;
		checkWin();
	}

	function checkLose(y: number, x: number) {
		if (grid[y][x].bomb) {
			game.state = "LOST";

			for (const [y, x] of game.bombs) {
				grid[y][x].flag = false;
				grid[y][x].isHidden = false;
			}
		}
	}

	function checkWin() {
		if (
			game.revealedNonBombs ===
			difficulty.gridSize * difficulty.gridSize - game.bombs.length
		) {
			game.state = "WON";
		}
	}

	function incrementTime() {
		if (game.state === "IN_GAME" && game.revealedNonBombs) {
			game.time++;
		}

		setTimeout(incrementTime, 1000);
	}
	incrementTime();

	function reset() {
		game = {
			flags: 0,
			time: 0,
			state: "IN_GAME",
			bombs: [],
			revealedNonBombs: 0,
		};
		grid = generateGrid();
	}
</script>

<main>
	<a
		target="_blank"
		rel="noopener"
		href="https://github.com/7USTIN/minesweeper"
	>
		<i class="material-icons">code</i>
	</a>

	<Header bind:selectedDiff {game} {difficulties} on:diffChange={reset} />

	<section
		style={`grid-template-columns: repeat(${difficulty.gridSize}, 1fr);`}
	>
		{#each grid as row, y (y)}
			{#each row as { bomb, flag, neighbours, isHidden }, x (x)}
				<Cell
					on:reveal={() => revealCells(y, x)}
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

	<button
		class="reset-btn"
		class:transparent={game.state === "IN_GAME"}
		on:click={reset}
	>
		<i class="material-icons">replay</i>
		<p>Restart</p>
	</button>
</main>

<style lang="scss">
	:global(:root) {
		--black: hsl(197, 8%, 8%);
		--white: hsl(241, 3%, 93%);
		--white-dark: hsl(241, 3%, 85%);
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
		position: relative;
	}

	section {
		user-select: none;
		-moz-user-select: none;
		-webkit-user-select: none;
		display: grid;
	}

	.reset-btn {
		border-radius: 7px;
		border: 0px solid var(--white);
		background: var(--white);
		color: var(--black);
		padding: 6px 10px;
		font-size: 16px;
		cursor: pointer;
		font-weight: 500;
		display: flex;
		align-items: center;
		justify-content: center;
		margin-top: 16px;

		i {
			font-size: 20px;
			margin-right: 8px;
		}

		&:hover {
			background: var(--white-dark);
		}
	}

	a {
		position: fixed;
		top: 32px;
		right: 64px;

		i {
			border-radius: 9999px;
			padding: 8px;
			color: var(--white);
			font-size: 24px;
		}

		&:hover i {
			background: hsla(241, 3%, 93%, 0.1);
		}
	}

	.transparent {
		opacity: 0;
	}
</style>
