<script lang="ts">
	export let difficulties: {};
	export let difficulty: {};
	export let time: number;

	let selectedDiff = 'Medium';

	$: difficulty = difficulties[selectedDiff];
</script>

<header>
	<select bind:value={selectedDiff}>
		{#each Object.keys(difficulties) as difficulty, idx (idx)}
			<option value={difficulty}>{difficulty}</option>
		{/each}
	</select>

	<div class="flags">
		<i class="material-icons">outlined_flag</i>
		<p>{difficulties[selectedDiff].flagCount}</p>
	</div>

	<div class="time">
		<i class="material-icons">timer</i>
		<p>{String(time).padStart(3, '0')}</p>
	</div>
</header>

<style lang="scss">
	header {
		display: flex;

		.time,
		.flags {
			display: flex;
			align-items: center;
		}
	}
</style>
