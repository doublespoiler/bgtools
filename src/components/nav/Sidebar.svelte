<script>
// @ts-nocheck

  import { slide } from 'svelte/transition';
  import { quintOut, backOut } from 'svelte/easing';
  import { clickOutside } from '../../lib/click_outside'
  import Nav from './Nav.svelte';

  export let show = false;

  const hide = () => {
    show = false;
    return null;
  }
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
</style>

{#if show}
<nav use:clickOutside on:outclick={() => (show = false)} transition:slide={{delay: 150, duration: 300, easing: quintOut, axis: 'x'}}>
  <Nav {hide} />
</nav>
{/if}
