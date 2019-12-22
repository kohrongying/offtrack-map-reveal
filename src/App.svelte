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
		{ id: 'School', position:0, value: 'School' },
		{ id: 'Town', position:0, value: 'Town' },
		{ id: 'CBD', position:0, value: 'CBD' },
		{ id: 'Airport', position:0, value: 'Airport'},    
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

<main id="landscape">
	<div class="map" id="map">
		<div id="dest-0" class="destination"></div>
		<div id="dest-1" class="destination"></div>
		<div id="dest-2" class="destination"></div>
		<div id="dest-3" class="destination"></div>
	</div>

	<div class="buttonArea">
		<img src="offtrack.png" alt="logo" width="90" />
		<button on:click={rotate(1, 90)}>Rotate Clockwise 90 degrees</button>
		<button on:click={rotate(-1, 90)}>Rotate Anti-Clockwise 90 degrees</button>
		<button on:click={rotate(1, 180)}>Rotate 180 degrees</button>
	</div>

	<div class="targetArea" id="outcomes">
		{#each OUTCOMES as outcome}
			<div class="outcomes" id={outcome.id}>
				{outcome.value}
			</div>
		{/each}
	</div>

	<div class="footer">
		You
	</div>
</main>

<style>
	:root {
		--square-width: 100px;
		--bg-color: #1A947E
	}
	main#landscape {
		background-color: var(--bg-color);
		height: 100vh;
		text-align: center;
		display: grid;
		grid-template-columns: 2fr 3fr 2fr;
		grid-template-rows: 6fr 1fr;
		grid-template-areas: "actionArea mapArea targetArea"
												"footer footer footer";
	}

	.footer {
		grid-area: footer;
		background-color: #FFC806;
		padding: 10px;
	}
	.buttonArea {
		grid-area: actionArea;
		align-self: center;
		padding: 15px;
	}
	button {
		border-radius: 20px;
		width: 100%;
		padding: 5px 15px;
	}

	.map {
		grid-area: mapArea;
		display: grid;
		align-self: center;
		justify-self: center;
		grid-template-columns: repeat(3, var(--square-width));
		grid-template-rows: repeat(3, var(--square-width));
		grid-gap: 2px;
	}
	.targetArea {
		grid-area: targetArea;
		align-self: center;
		justify-self: center;
		display: grid;
		grid-gap: 10px;
		grid-template-rows: repeat(2, var(--square-width));
		grid-template-columns: repeat(2, var(--square-width));
	}
	.destination {
		width: var(--square-width);
		height: var(--square-width);
		border: 2px white dashed;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	.outcomes {
		width: calc(var(--square-width) - 4px);
		height: calc(var(--square-width) - 4px);
		background: #00C6E0;
		display: flex;
		align-items: center;
		justify-content: center;
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

	@media (max-width: 900px) {
		:root {
			--square-width: 80px;
		}
	}

	@media screen and (orientation: portrait) {
		main#landscape {
			transform: rotate(-90deg);
			transform-origin: left top;
			width: 100vh;
			height: 100vw;
			overflow-x: hidden;
			position: absolute;
			top: 100%;
			left: 0; 
		}
	}
</style>