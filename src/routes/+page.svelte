<script lang="ts">
	import LedDigit from '../lib/LedDigit.svelte';
</script>

<div class="container">
	{#each Array(4) as _, i}
		<LedDigit />
	{/each}
</div>

<style>
	.container {
		display: flex;
		flex-wrap: wrap;
		gap: 10px;
	}
</style>
