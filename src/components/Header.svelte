<script lang="ts">
	import { createEventDispatcher } from "svelte";

	export let difficulties: {};
	export let selectedDiff: string;
	export let game: { flags: number; time: number };

	const dispatch = createEventDispatcher();

	let showDiffSelection = false;

	function handleSelection(difficulty: string) {
		selectedDiff = difficulty;
		localStorage.setItem("selectedDiff", selectedDiff);
		dispatch("diffChange");
	}
</script>

<header>
	<div
		class="difficulty"
		on:click={() => (showDiffSelection = !showDiffSelection)}
	>
		<p>{selectedDiff}</p>
		<i class="material-icons">expand_more</i>

		{#if showDiffSelection}
			<div class="diff-selection">
				{#each Object.keys(difficulties) as diff, idx (idx)}
					<div on:click={() => handleSelection(diff)}>
						<i
							class="material-icons"
							class:selected={selectedDiff === diff}
						>
							done
						</i>
						<p>{diff}</p>
					</div>
				{/each}
			</div>
		{/if}
	</div>

	<div class="info">
		<div class="flags">
			<i class="material-icons">flag</i>
			<p>{difficulties[selectedDiff].flags - game.flags}</p>
		</div>

		<div class="time">
			<i class="material-icons">timer</i>
			<p>{String(game.time).padStart(3, "0")}</p>
		</div>
	</div>
</header>

<style lang="scss">
	header {
		display: flex;
		justify-content: space-between;
		user-select: none;
		-moz-user-select: none;
		-webkit-user-select: none;
		margin-bottom: 16px;
		width: 100%;

		.difficulty {
			border-radius: 7px;
			border: 0px solid var(--white);
			background: var(--white);
			color: var(--black);
			padding: 6px 10px;
			cursor: pointer;
			display: flex;
			align-items: center;
			position: relative;
			font-weight: 500;

			i {
				font-size: 18px;
				margin-left: 8px;
			}

			.diff-selection {
				position: absolute;
				top: 100%;
				left: 0;
				z-index: 5;
				background: var(--white);
				border-radius: 7px;
				overflow: hidden;
				box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;

				div {
					display: flex;
					align-items: center;
					width: 100%;
					padding: 6px 0;

					i {
						font-size: 18px;
						margin: 0 6px 0 8px;
						opacity: 0;
					}

					.selected {
						opacity: 1;
					}

					p {
						margin-right: 20px;
						font-weight: 500;
					}

					&:hover {
						background: var(--white-dark);
					}
				}
			}
		}

		.info {
			display: flex;

			.time,
			.flags {
				display: flex;
				align-items: center;
				margin-left: 16px;

				p {
					margin-left: 2px;
					font-size: 16px;
				}
			}
		}
	}
</style>
