<script lang="ts">
	import { onDestroy } from 'svelte';

	const DIGITS = [
		[1, 1, 1, 0, 1, 1, 1],
		[0, 0, 1, 0, 0, 1, 0],
		[1, 0, 1, 1, 1, 0, 1],
		[1, 0, 1, 1, 0, 1, 1],
		[0, 1, 1, 1, 0, 1, 0],
		[1, 1, 0, 1, 0, 1, 1],
		[1, 1, 0, 1, 1, 1, 1],
		[1, 1, 1, 0, 0, 1, 0],
		[1, 1, 1, 1, 1, 1, 1],
		[1, 1, 1, 1, 0, 1, 1]
	];
	const intervalTimeMin = 1000;
	const intervalTimeMax = 3000;

	let currentDigit: number = generateRandomDigit();

	function generateRandomDigit(): number {
		return Math.floor(Math.random() * 10);
	}

	function generateRandomMilliseconds(min: number, max: number): number {
		return Math.floor(Math.random() * (max - min + 1)) + min;
	}

	function isOn(current: number, segment: number): boolean {
		return !!DIGITS[current][segment];
	}

	function updateDigit(): void {
		currentDigit = generateRandomDigit();
	}

	const intervalId = setInterval(
		() => updateDigit(),
		generateRandomMilliseconds(intervalTimeMin, intervalTimeMax)
	);

	onDestroy(() => clearInterval(intervalId));
</script>

<div class="led-digit">
	{#each Array(7) as _, i}
		<div class="led-segment segment-{i}" class:on={isOn(currentDigit, i)} />
	{/each}
</div>

<style>
	.led-digit {
		width: 50px;
		height: 80px;
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
		align-items: center;
		position: relative;
	}
	.led-segment {
		width: 10px;
		height: 10px;
		background-color: #ffffff;
		border-radius: 5px;
		margin: 2px;
		position: absolute;
	}
	.segment-0 {
		width: 40px;
		left: 3px;
		top: 0;
	}
	.segment-1 {
		height: 40px;
		left: 0;
		top: 4px;
	}
	.segment-2 {
		height: 40px;
		right: 0;
		top: 4px;
	}
	.segment-3 {
		width: 40px;
		left: 3px;
		top: 40px;
	}
	.segment-4 {
		height: 40px;
		left: 0;
		top: 44px;
	}
	.segment-5 {
		height: 40px;
		right: 0;
		top: 44px;
	}
	.segment-6 {
		width: 40px;
		left: 3px;
		top: 80px;
	}
	.on {
		background-color: #ff0000;
	}
</style>
