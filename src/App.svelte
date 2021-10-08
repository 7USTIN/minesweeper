<script lang="ts">
</script>

<main>
	<h1>Hello World</h1>
</main>

<style lang="scss">
	:global(:root) {
		--bg: #ffffff;
		--text: #000000;
	}

	:global(*) {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
		outline: none;
	}

	:global(html) {
		scroll-behavior: smooth;
	}

	:global(body, button, input) {
		font-family: 'Inter', sans-serif;
		font-size: 14px;
		background: var(--bg);
		color: var(--text);
	}
</style>
