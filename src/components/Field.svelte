<script lang="ts">
	export let bomb: string;
	export let grid: string[];
	export let fieldIdx: number;
	export let rowIdx: number;

	$: neighbours = bomb ? -1 : getNeighbours(grid);

	function getNeighbours(grid: string[]): number {
		let neighbours = 0;

		for (let i = -1; i < 2; i++) {
			for (let j = -1; j < 2; j++) {
				if ([-1, grid.length].includes(rowIdx + i)) {
					continue;
				}

				if (grid[rowIdx + i][fieldIdx + j]) {
					neighbours++;
				}
			}
		}

		return neighbours;
	}
</script>

<div class="wrapper">
	<div class="field" class:bomb>
		{#if neighbours > 0}
			{neighbours}
		{/if}
	</div>
</div>

<style lang="scss">
	.wrapper {
		cursor: pointer;
		width: 30px;
		height: 30px;
		display: flex;
		align-items: center;
		justify-content: center;

		.bomb {
			background: var(--gray);
		}

		.field {
			display: flex;
			align-items: center;
			justify-content: center;
			width: 92.5%;
			height: 92.5%;
			border-radius: 7px;
			border: 1px solid var(--gray);
		}
	}
</style>
