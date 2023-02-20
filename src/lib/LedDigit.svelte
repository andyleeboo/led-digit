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

<filter id="sofGlow" height="300%" width="300%" x="-75%" y="-75%">
	<!-- Thicken out the original shape -->
	<feMorphology operator="dilate" radius="4" in="SourceAlpha" result="thicken" />

	<!-- Use a gaussian blur to create the soft blurriness of the glow -->
	<feGaussianBlur in="thicken" stdDeviation="10" result="blurred" />

	<!-- Change the colour -->
	<feFlood flood-color="rgb(0,186,255)" result="glowColor" />

	<!-- Color in the glows -->
	<feComposite in="glowColor" in2="blurred" operator="in" result="softGlow_colored" />

	<!--	Layer the effects together -->
	<feMerge>
		<feMergeNode in="softGlow_colored" />
		<feMergeNode in="SourceGraphic" />
	</feMerge>
</filter>
<svg width="66" height="124" viewBox="0 0 66 124" fill="none" xmlns="http://www.w3.org/2000/svg">
	<path
		filter="url(#sofGlow)"
		d="M6 0C4.30872 0 2.78088 0.699772 1.69022 1.82558L14.6255 14H51.3745L64.3098 1.82558C63.2191 0.699772 61.6913 0 60 0H6Z"
		fill="currentColor"
	/>
	<path
		d="M13.7426 55L3.80258 62L13.7426 69H52.2574L62.1974 62L52.2574 55H13.7426Z"
		fill="currentColor"
	/>
	<path
		d="M13 14.5299L0.783997 3.03254C0.285065 3.90767 0 4.92054 0 6V59.4348L2.42001 61.139L13 53.6883V14.5299Z"
		fill="currentColor"
	/>
	<path
		d="M6 124C4.30872 124 2.78088 123.3 1.69022 122.174L14.6255 110H51.3745L64.3098 122.174C63.2191 123.3 61.6913 124 60 124H6Z"
		fill="currentColor"
	/>
	<path
		d="M13 109.47L0.783997 120.967C0.285065 120.092 0 119.079 0 118V64.5652L2.42001 62.861L13 70.3117V109.47Z"
		fill="currentColor"
	/>
	<path
		d="M53 14.5299L65.216 3.03254C65.7149 3.90767 66 4.92054 66 6V59.4348L63.58 61.139L53 53.6883V14.5299Z"
		fill="currentColor"
	/>
	<path
		d="M53 109.47L65.216 120.967C65.7149 120.092 66 119.079 66 118V64.5652L63.58 62.861L53 70.3117V109.47Z"
		fill="currentColor"
	/>
</svg>
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
		background-color: #333;
		border-radius: 5px;
		margin: 2px;
		position: absolute;
		box-shadow: 0 0 50px 15px #ff000005;
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
		z-index: 1000;
		box-shadow: 0 0 50px 15px #ff000005;
	}
</style>
