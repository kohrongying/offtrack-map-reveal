<script>
	import dragula from 'dragula';
	import '../node_modules/dragula/dist/dragula.min.css';
	import { onMount } from 'svelte';
	onMount(() => {
		dragula([
			document.getElementById('dest-0'),
			document.getElementById('dest-1'),
			document.getElementById('dest-2'),
			document.getElementById('dest-3'),
			document.getElementById('outcomes')
		], {
			revertOnSpill: true,
		}).on('drop', (el, target) => {
			const dest = target.getAttribute('id');
			const outcome = el.getAttribute('id')
			setOutcome(outcome, dest.slice(-1))
		})
	})

	const OUTCOMES = [
		{id:'School',position:0, value: 'School'},
		{id:'Town',position:0, value: 'Town'},
		{id:'CBD',position:0, value: 'CBD'},
		{id:'Airport',position:0, value: 'Airport'},
	]

	const rotate = (direction, degrees) => () => {
		let currValues = ['','','','']
		OUTCOMES.forEach(x => {
			currValues[x.position] = x.value
		})
		let numRotations = Math.floor(degrees/90);
		if (direction === 1) { // clockwise
			for (let i = 0; i < numRotations; i+= 1) {
				const popped = currValues.pop()
				currValues.unshift(popped)
			}
		} else { // anticlockwise
			for (let i = 0; i < numRotations; i+= 1) {
				const popped = currValues.shift()
				currValues.push(popped)
			}
		}
		for (let i=0; i<OUTCOMES.length; i+=1) {
			OUTCOMES[i].value = currValues[OUTCOMES[i].position]
		}
	}

	const setOutcome = (outcome, destIndex) => {
		OUTCOMES.forEach(x => {
			if (x.id === outcome) {
				x.position = parseInt(destIndex)
			}
		})
	}
</script>

<main>
	<h1>Offtrack Map!</h1>
	<div class="map" id="map">
		<div id="dest-0" class="destination"></div>
		<div id="dest-1" class="destination"></div>
		<div id="dest-2" class="destination"></div>
		<div id="dest-3" class="destination"></div>
	</div>

	<button on:click={rotate(1, 90)}>Rotate Clockwise 90 degrees</button>
	<button on:click={rotate(-1, 90)}>Rotate Anti-Clockwise 90 degrees</button>
	<button on:click={rotate(1, 180)}>Rotate 180 degrees</button>

	<div id="outcomes">
		{#each OUTCOMES as outcome}
			<div class="destination outcomes" id={outcome.id}>
				{outcome.value}
			</div>
		{/each}
	</div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}
	
	.map {
		display: grid;
		grid-template-columns: repeat(3, 50px);
		grid-template-rows: repeat(3, 50px);
	}

	.destination {
		width: 50px;
		height: 50px;
		background: red;
		display: flex;
		align-items: center;
		justify-content: center;
		z-index: 99;
	}
	.outcomes {
		background: turquoise;
		z-index: 1;
	}

	#dest-0 {
		grid-column-start: 2;
		grid-column-end: 3;
		grid-row-start: 1;
		grid-row-end: 2;
	}
	#dest-3 {
		grid-column-start: 1;
		grid-column-end: 2;
		grid-row-start: 2;
		grid-row-end: 3;
	}
	#dest-2 {
		grid-column-start: 2;
		grid-column-end: 3;
		grid-row-start: 3;
		grid-row-end: 4;
	}
	#dest-1 {
		grid-column-start: 3;
		grid-column-end: 4;
		grid-row-start: 2;
		grid-row-end: 3;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>