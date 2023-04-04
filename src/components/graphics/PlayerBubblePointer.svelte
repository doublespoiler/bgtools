<script>
  import { onMount } from "svelte";
  import { spring } from 'svelte/motion';


  /**
   * @type {number}
   * 
   */
  export let id;
  /**
   * @type {{
   *   identifier: number;
   *   pageX: number;
   *   pageY: number;
   *   clientX: number;
   *   clientY: number;
   *   screenX: number;
   *   screenY: number;
   * }}
   */
   export let touch;

  export let coords = spring({ x: touch.clientX, y: touch.clientY}, {
    stiffness: 0.1,
    damping: 0.25
  })
  let size = spring(10);

  onMount(()=> {
    console.log(`Mounted touch ${touch.identifier}`)
  })


</script>

<style>
  svg {
    width: 100%;
    height: 100%;
    position: absolute;
    left: 0;
    top: 0;
  }

  circle{
    fill: #ff3e00;
  }

  div {
    width: 20px;
    height: 20px;
    background-color: green;
  }
</style>


<svg id="touch{touch.identifier}"
  on:pointermove={(e)=>{
    console.log(e.clientX)
    coords.set({x: e.clientX, y: e.clientY})
  }}
>
  <circle 
    cx={$coords.x}
    cy={$coords.y}
    r={$size}
    />
    <text>{$coords.x}, {$coords.y}</text>
</svg>
