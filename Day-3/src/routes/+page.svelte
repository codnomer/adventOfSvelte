<script>
	import { onMount } from 'svelte';

	let presents = [];

	onMount(async () => {
		try {
			const response = await fetch('https://advent.sveltesociety.dev/data/2023/day-three.json');
			presents = await response.json();
			console.log(presents);
		} catch (error) {
			console.error('Error fetching data:', error);
		}
	});

	let selectedPresents = [];

	function calculateSleighLoad() {
		return selectedPresents.reduce((totalWeight, present) => totalWeight + present.weight, 0);
	}

	function isOverweight() {
		return calculateSleighLoad() > 100;
	}

	function selectPresent(event, present) {
		event.stopPropagation();
		const newTotalWeight = calculateSleighLoad() + present.weight;

		if (newTotalWeight <= 100) {
			selectedPresents = [...selectedPresents, present];
		} else {
			console.warn('Adding this present would exceed the weight limit!');
		}
	}

	function deselectPresent(event, present) {
		event.stopPropagation();
		selectedPresents = selectedPresents.filter((selected) => selected !== present);
	}
</script>

<main>
	<h1>Sleigh Load Balancer</h1>

	<div>
		<h2>Available Presents</h2>
		<ul>
			{#each presents as present, index (index)}
				<li on:click|stopPropagation={(e) => selectPresent(e, present)}>
					{present.name} - {present.weight} kg
				</li>
			{/each}
		</ul>
	</div>

	<div>
		<h2>Selected Presents</h2>
		<ul>
			{#each selectedPresents as present, index (index)}
				<li on:click|stopPropagation={(e) => deselectPresent(e, present)}>
					{present.name} - {present.weight} kg
				</li>
			{/each}
		</ul>
		<p class:overweight={isOverweight()}>
			{isOverweight() ? 'Sleigh is overweight!' : 'Sleigh load is acceptable.'}
		</p>
		<p>Total Sleigh Load: {calculateSleighLoad()} kg</p>
	</div>
</main>

<style>
	.overweight {
		color: red;
	}
</style>
