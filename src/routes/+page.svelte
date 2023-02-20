<script lang="ts">
	import LedDigit from '../lib/LedDigit.svelte';
</script>

<div class="container">
	{#each Array(4) as _, i}
		<LedDigit />
	{/each}
</div>

<style>
	:global(body) {
		margin: 0;
		padding: 0;
		background-color: #1a1a1a;
	}
	.container {
		display: flex;
		flex-wrap: wrap;
		gap: 10px;
		padding: 20px;
	}
</style>
