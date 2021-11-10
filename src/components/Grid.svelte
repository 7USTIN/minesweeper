<script lang="ts">
	import Field from "./Field.svelte";

	export let difficulties: {};
	export let selectedDiff: string;
	export let time: number;

	$: difficulty = difficulties[selectedDiff];
	$: grid = generateGrid(difficulty);

	function randomIndex(array: any[]): number {
		return (array.length * Math.random()) | 0;
	}

	function generateGrid(difficulty): [][] {
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

		for (let i = 0; i < difficulty.flagCount; i++) {
			let randomNum = [randomIndex(grid), randomIndex(grid)];

			if (grid[randomNum[0]][randomNum[1]].bomb) {
				i--;
			} else {
				grid[randomNum[0]][randomNum[1]] = {
					bomb: true,
					flag: false,
					neighbours: 0,
				};

				for (let i = -1; i < 2; i++) {
					for (let j = -1; j < 2; j++) {
						if ([-1, grid.length].includes(randomNum[0] + i)) {
							continue;
						}

						if (grid[randomNum[0] + i][randomNum[1] + j]) {
							grid[randomNum[0] + i][randomNum[1] + j]
								.neighbours++;
						}
					}
				}
			}
		}

		return grid;
	}
</script>

<section>
	<div style={`grid-template-columns: repeat(${difficulty.gridSize}, 1fr)`}>
		{#each grid as row, rowIdx (rowIdx)}
			{#each row as { bomb, flag, neighbours }, fieldIdx (fieldIdx)}
				<Field bind:flag {bomb} {neighbours} />
			{/each}
		{/each}
	</div>
</section>

<style lang="scss">
	section {
		user-select: none;
		-moz-user-select: none;
		-webkit-user-select: none;

		div {
			display: grid;
		}
	}
</style>
