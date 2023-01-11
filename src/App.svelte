<script>
	let drData = [], filtered = []
	let count = 0
	fetch('./deathRow.json')
		.then(response => response.json())
		.then(data => {
			drData = data
			filtered = drData
			count = filtered.length
			//make an array of all properties with race and log it to console
			console.log(drData.map(item => item['Race']))
		});

		let searchName, race

		const filterItemsByName = () => {
			filtered = drData.filter(item => {
				return item["First Name"].toLowerCase().includes(searchName.toLowerCase())
			})
		} 
		const filterItemsByRace = () => {
			console.log('filtering by ' + race)
			if(race==='*') return (filtered = drData)
			filtered = drData.filter(item => {
				return item["Race"].toLowerCase().includes(race.toLowerCase())
			})
		}

		$: count = filtered.length
</script>

<main>
	<div class="filter">
		<label for="search">Search by name</label>
		<input type="text" bind:value={searchName} on:input={filterItemsByName}>

		<label for="race">Filter by race</label>
		<select id='race' name="race" bind:value={race} on:change={filterItemsByRace}>
			<option value="*">select race</option>
			<option value="black">black</option>
			<option value="white">white</option>
			<option value="hispanic">hispanic</option>
		</select>
		<div class="count">Prisoner count:</div>
		<div>{count}</div>
	</div>
	<div class="prisoners">
		{#if filtered}
			 {#each filtered as item}
			 <div class="prisoner">
				 <h1>{item["First Name"]}</h1>
				 <div>Age at execution: {item["Age at Execution"]}</div>
				 <div class="last">{item["Last Statement"]}</div>
			 </div>
			 {/each}
		{/if}
	</div>

</main>

<style>
	:global(body){
		background-color: #333;
		color:white;
		margin: 0;
		padding: 0;
		box-sizing: border-box;
	}
	.filter{
		background-color: black;
		width:100vw;
		padding:1rem;
		box-sizing: border-box;
		color:whitesmoke;
		display: grid;
		grid-template-columns: 3fr 8fr;
	}
	.prisoners {
		display:grid;
		grid-template-columns: repeat(auto-fill, minmax(400px, 1fr));
		gap:.5rem;
	}

	.prisoner {
		background: #aaa;
		padding: 1em;
		margin: 1em;
		color:white;
	}
	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-weight: 100;
		margin:0;
	}
	.last{
		font-style: italic;
		color:#555;
		margin:.5rem;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>