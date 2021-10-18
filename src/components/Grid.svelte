<script lang="ts">
	type difficulty = {
		flagCount: number;
		gridSize: number;
	};

	export let difficulty: difficulty;
	export let time: number;

	function randomIndex(array: any[]): number {
		return (array.length * Math.random()) | 0;
	}

	function generateGrid(difficulty: difficulty): string[] {
		let grid = [];

		for (let i = 0; i < difficulty.gridSize; i++) {
			grid.push([]);

			for (let j = 0; j < difficulty.gridSize; j++) {
				grid[i].push('');
			}
		}

		for (let i = 0; i < difficulty.flagCount; i++) {
			let randomNum = [randomIndex(grid), randomIndex(grid)];

			if (grid[randomNum[0]][randomNum[1]]) {
				i--;
			} else {
				grid[randomNum[0]][randomNum[1]] = 'X';
			}
		}

		return grid;
	}

	$: grid = generateGrid(difficulty);

	$: console.log(...grid);
</script>

<section>
	<h1>Grid</h1>
</section>

<style lang="scss">
</style>
