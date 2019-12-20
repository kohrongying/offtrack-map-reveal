<script>
	let destinations = [
		{ position: 1, value: 1 },
		{ position: 2, value: 2 },
		{ position: 3, value: 3 },
		{ position: 4, value: 4 },
	]

	const OUTCOMES = [
		'School',
		'Town',
		'CBD',
		'Airport'
	]

	const rotate = (direction, degrees) => () => {
		let currValues = destinations.map(x => x.value)
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
		for (let i = 0; i < destinations.length; i+= 1) {
			destinations[i].value = currValues[i]
		}
	}

	const setOutcome = () => {

	}

	const handleDragStart = (event) => {
		console.log('hi')
		event
			.currentTarget
			.style
			.backgroundColor = 'yellow';
	}

	const handleDrop = (event) => {
		const id = event.dataTransfer.getData('text');
		console.log(event)
	console.log('dest', event.srcElement.id)
	}
</script>

<main>
	<h1>Offtrack Map!</h1>
	<div class="map">
		{#each destinations as dest}
			<div
				id={`dest-${dest.position}`}
				class="destination"
				on:dragover={(event) => event.preventDefault()}
				on:drop={handleDrop}
			>
				{dest.value}
			</div>
		{/each}
	</div>

	<button on:click={rotate(1, 90)}>Rotate Clockwise 90 degrees</button>
	<button on:click={rotate(-1, 90)}>Rotate Anti-Clockwise 90 degrees</button>
	<button on:click={rotate(1, 180)}>Rotate 180 degrees</button>

	{#each OUTCOMES as outcome}
		<div class="destination outcomes" id={outcome} draggable={true} on:dragstart={handleDragStart}>
			{outcome}
		</div>
	{/each}
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
		/* grid-gap: 1rem; */
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
	}
	.outcomes {
		background: turquoise;
	}

	#dest-1 {
		grid-column-start: 2;
		grid-column-end: 3;
		grid-row-start: 1;
		grid-row-end: 2;
	}
	#dest-4 {
		grid-column-start: 1;
		grid-column-end: 2;
		grid-row-start: 2;
		grid-row-end: 3;
	}
	#dest-3 {
		grid-column-start: 2;
		grid-column-end: 3;
		grid-row-start: 3;
		grid-row-end: 4;
	}
	#dest-2 {
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