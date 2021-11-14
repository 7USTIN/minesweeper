<script lang="ts">
	import { createEventDispatcher } from "svelte";

	export let bomb: boolean;
	export let flag: boolean;
	export let isHidden: boolean;
	export let neighbours: number;
	export let difficulty: { flags: number; gridSize: number };
	export let game: { time: number; flags: number; state: string };

	const dispatch = createEventDispatcher();

	function toggleFlag() {
		if (game.state !== "IN_GAME") {
			return;
		}

		if (flag) {
			flag = false;
			game.flags--;
		} else if (difficulty.flags - game.flags && isHidden) {
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
	<div class="cell" class:isHidden>
		{#if neighbours > 0 && !bomb && !flag && !isHidden}
			{neighbours}
		{/if}

		{#if flag}
			<i class="material-icons">flag</i>
		{/if}

		{#if bomb && !isHidden}
			<i class="material-icons">coronavirus</i>
			<div class="circle" />
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

		.cell {
			display: flex;
			align-items: center;
			justify-content: center;
			width: 92.5%;
			height: 92.5%;
			border-radius: 7px;
			border: 2px solid var(--gray-dark);
			font-weight: 500;

			.circle {
				border-radius: 9999px;
				width: 8px;
				height: 8px;
				background: var(--white);
				position: absolute;
			}

			i {
				font-size: 18px;
			}
		}
	}
</style>
