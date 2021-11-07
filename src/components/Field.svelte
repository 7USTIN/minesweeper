<script lang="ts">
	export let bomb: string;
</script>

<div class="wrapper">
	<div class="field" class:bomb>
		
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
