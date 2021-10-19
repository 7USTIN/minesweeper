<script lang="ts">
	import Field from './Field.svelte';

	export let difficulties: {};
	export let selectedDiff: string;
	export let time: number;

	$: difficulty = difficulties[selectedDiff];
	$: grid = generateGrid(difficulty);

	function randomIndex(array: any[]): number {
		return (array.length * Math.random()) | 0;
	}

	function generateGrid(difficulty): string[] {
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
				grid[randomNum[0]][randomNum[1]] = '1';
			}
		}

		return grid;
	}
</script>

<section>
	<div style={`grid-template-columns: repeat(${difficulty.gridSize}, 1fr)`}>
		{#each grid as row, r (r)}
			{#each row as field, f (f)}
				<Field bomb={field} />
			{/each}
		{/each}
	</div>
</section>

<style lang="scss">
	section {
		div {
			display: grid;
		}
	}
</style>
