<script>
// @ts-nocheck

	import { fly, fade } from "svelte/transition";
	let result = [];
	let selectedDie = 20;
	let diceToRoll = 1;
	let cols = "100%";

	async function clear(){
		let blankArr = [];
		result=[...blankArr];
		cols = "100%";
		console.log('clear done')
	}

  async function roll(num) {
		await calcCols();
		
    // result = Math.floor(Math.random() * num) + 1;
		while(result.length < diceToRoll){
			result = [...result, (Math.floor(Math.random() * num) + 1)]
		}
		
  }

	async function calcCols() {
		await clear();
		const modThree = Math.floor(diceToRoll % 3);
		let columns = "";
		console.log("dicetoroll", diceToRoll)
		if(diceToRoll == 1){
			columns = "100%";
		} else if(diceToRoll == 2){
			columns = "50% 50%";
		} else {
			columns = "33% 33% 33%";
		}
		cols = columns;
		console.log('calccols done', cols)
	}

	const handleSelectDie = (num) => {
		const buttonPressed = document.getElementsByClassName(`d${num}`)[0];
		console.log(buttonPressed)
		document.getElementById("selected-die").removeAttribute("id");
		selectedDie = num;
		result = [];
		buttonPressed.setAttribute("id", "selected-die");
		console.log(buttonPressed)
	}
</script>

<style lang="scss">
	.display{
		font-size: 5rem;
	}

	.roll-select{
		margin-bottom: 0.5rem;
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 0.5rem;
		font-size: 2.5rem;
		button{
			font-size: 2.5rem;
			width: 7rem;
		}
		input{
			font-size: 2.5rem;
			width: 7rem;
			text-align: center;
		}
	}
	.dice-roller{
		margin: auto;
		height:95%;
		display: flex;
		flex-direction: column;
		align-items: center;
		padding: 0.5rem;
	}

	.dice-select{
		display: grid;
		grid-template-columns: 25% 25% 25% 25%;
		gap: 0.35rem;
	}



	.dice-select-button{
		font-size: 1.5rem;
	}

	.display-grid{
		display: grid;
		place-items: center;
		grid-template-columns:var(--grid-template-columns);
		font-size: 2.5rem;
		gap: auto;
		row-gap: 1rem;
		text-align: center;
		min-width: 80%;
		margin: auto;
		overflow-y:scroll;

	}

	#selected-die{
		background-color: #666;
		color: white;
	}

</style>

<div class="dice-roller">

	<!-- <p class="display">
		
		{result.length > 1 ? `${result.length}`: " "}D{selectedDie} 
		{result.length !== 0 ? `= ${result}` : " "}
		{#if result.length > 1}
			= {result.reduce((a, b) => a + b, 0)}
		{/if}
	</p> -->
	<div class="display">
		{diceToRoll}d{selectedDie} {result.length > 0 ? ` = ${result.reduce((a, b) => a + b, 0)}` : ""}
	</div>
	<div class="display-grid" style="--grid-template-columns: {cols}">
		{#each result as roll, i }
			<div class="result-number" in:fly={{delay:(result.length > 8 ? ( 50 * i) : (100 * i)), y:200, duration:1000}}>
				{roll}
			</div>
		{/each}
	</div>
	
	

	<div class="roll-select">
		<input type=number bind:value={diceToRoll} min=1><button class="roll-button" on:click={roll(selectedDie)}>Roll!</button>
	</div>
	

	<div class="dice-select">
		<button class="dice-select-button d2" on:click={() => handleSelectDie(2)}>d2</button> 
		<button class="dice-select-button d4" on:click={() => handleSelectDie(4)}>d4</button> 
		<button class="dice-select-button d6" on:click={() => handleSelectDie(6)}>d6</button>
		<button class="dice-select-button d8" on:click={() => handleSelectDie(8)}>d8</button>
		<button class="dice-select-button d10" on:click={() => handleSelectDie(10)}>d10</button>
		<button class="dice-select-button d12" on:click={() => handleSelectDie(12)}>d12</button>
		<button class="dice-select-button d20" id="selected-die" on:click={() => handleSelectDie(20)}>d20</button>
		<button class="dice-select-button d100" on:click={() => handleSelectDie(100)}>d100</button>
	</div>
</div>



