<script>
  let players = [
    { id: 1, name: 'player 1', life: 20, poison: 0 },
    { id: 2, name: 'player 2', life: 20, poison: 0 }
  ]

  const addPlayer = () => {
    players = [...players, {id: players.length + 1, name: `player ${players.length + 1}`, life: 20, poison: 0}];
  }

  $: activePlayers = players;
</script>

<style>
  input{
    background: none;
    border: none;
    font-size: 5em;
    width: 2em;
    text-align: center;
  }
  /* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type=number] {
  -moz-appearance: textfield;
  appearance: textfield;
}

.grid-container{
  display: grid;
  grid-template-columns: auto auto;
  gap: 0.5rem;
}
</style>

<h1>Life Counter</h1>

<div class="grid-container">
  {#each activePlayers as player (player.id)}
  <div class="playerControl">
    <h2 contenteditable="true" bind:innerHTML={player.name}></h2>
    <input type=number bind:value={player.life} min=0>
    <input type=number bind:value={player.poison} min=0>
  </div>
{/each}
</div>


<button on:click={addPlayer}>Add Player</button>




