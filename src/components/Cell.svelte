<script lang="ts">
	import { createEventDispatcher } from "svelte";

	export let bomb: boolean;
	export let flag: boolean;
	export let isHidden: boolean;
	export let neighbours: number;
	export let difficulty: { flags: number; gridSize: number };
	export let game: { time: number; flags: number };

	const dispatch = createEventDispatcher();

	function toggleFlag() {
		if (flag) {
			flag = false;
			game.flags--;
		} else if (difficulty.flags - game.flags) {
			flag = true;
			game.flags++;
		}
	}
</script>

<div
	class="wrapper"
	on:click={() => dispatch("reveal")}
	on:contextmenu|preventDefault={toggleFlag}
>
	<div class="cell" class:bomb class:isHidden>
		{#if neighbours > 0 && !bomb && !flag && !isHidden}
			{neighbours}
		{/if}

		{#if flag}
			<i class="material-icons">flag</i>
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

		.isHidden {
			background: var(--gray-dark) !important;
			border: 0px solid transparent !important;
		}

		.bomb {
			background: lightgrey;
		}

		.cell {
			display: flex;
			align-items: center;
			justify-content: center;
			width: 92.5%;
			height: 92.5%;
			border-radius: 7px;
			border: 2px solid var(--gray-dark);
			font-weight: 500;

			i {
				font-size: 18px;
			}
		}
	}
</style>
