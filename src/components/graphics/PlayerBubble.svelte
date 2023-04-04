<script>
  import { onMount } from "svelte";
  import { spring } from 'svelte/motion';



  /**
   * @type {{
   *   identifier: number;
   *   pageX: number;
   *   pageY: number;
   *   clientX: number;
   *   clientY: number;
   *   screenX: number;
   *   screenY: number;
   *   picked: boolean;
   * }}
   */
  export let touch = {
    identifier: 0,
    pageX: 0,
    pageY: 0,
    clientX: 0,
    clientY: 0,
    screenX: 0,
    screenY: 0,
    picked: false
  };

  let coords = spring({ x: touch.clientX, y: touch.clientY}, {
    stiffness: 0.1,
    damping: 0.25
  })

  $: size = touch.picked ? spring(60) : spring(40);


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
    fill: #444;
  }

  .heavy{
    font: bold 30px sans-serif;
    fill: white;
  }
</style>


<svg class="touch{touch.identifier}">
  <circle 
    cx={touch.clientX}
    cy={touch.clientY}
    r={$size}
    style="fill: {touch.picked ? 'red' : '#444'}"
    />
    <text
      x={touch.clientX}
      y={touch.clientY}
      class="heavy"
    >{touch.identifier}</text>
</svg>
