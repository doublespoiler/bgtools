<script>
  import { slide } from 'svelte/transition';
  import { quintOut, backOut } from 'svelte/easing';
  import { clickOutside } from '../../lib/click_outside'


  export let show = false;
</script>

<style>
  nav {
    position: fixed;
    top: 0;
    right: 0;
    height: 100%;
    padding: 2rem 1rem 0.6rem;
    border-left: 1px solid #aaa;
    background: #fff;
    overflow-y: auto;
    width: 10rem;
  }
  li{
			list-style: none;
	}
</style>

{#if show}
<nav use:clickOutside on:outclick={() => (show = false)} transition:slide={{delay: 150, duration: 300, easing: quintOut, axis: 'x'}}>
  <ul on:keydown={() => (show = false)} on:click={() => (show = false)}>
		<li><a href="/life">Life Counter</a></li>
		<li><a href="/dice">Dice Roller</a></li>
		<li><a href="/picker">Player Picker</a></li>
	</ul>
</nav>
{/if}
