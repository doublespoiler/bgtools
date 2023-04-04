<script>
  

// @ts-nocheck

  import { json } from '@sveltejs/kit';
  import { onMount } from 'svelte';
  import PlayerBubble from '../../components/graphics/PlayerBubble.svelte';
  import PlayerBubblePointer from '../../components/graphics/PlayerBubblePointer.svelte';

  let logMsg = "loaded \n";
  let allCapturedPoints = [];
  let allTouches = [{
    identifier: -1,
    pageX: 0,
    pageY: 0,
    clientX: 0,
    clientY: 0,
    screenX: 0,
    screenY: 0
  }];
  let capturedPointerId = null;
  let x = 0;
  let y = 0;
  let timer = 0;
  let timerGoing = false;
  let interval = null;
  let pickedPlayer = null;

  const startTimer = () => {
    if (interval){
      clearInterval(interval)
      timerGoing = false;
      timer = 0;
    }
    interval = setInterval(() => {
      timerGoing = true;
      timer = timer + 1;
      log(`timer ${timer}`)
      if(timer > 3){
        pickPlayer();
        timer = 0;
        clearInterval(interval);
        timerGoing = false;
      }
    }, 1000);
  }

  const pickPlayer = () => {
    let allTouchesStr = allTouches.map((touch) => touch.identifier)
    log(`picking from ${allTouchesStr}`)
    const randIndex = Math.floor(Math.random() * (allTouches.length - 1)) + 1;
    pickedPlayer = allTouches.filter((touch) => touch.identifier === allTouches[randIndex].identifier)
    let pickedPlayerIndex = touchIndexById(pickedPlayer[0].identifier);
    allTouches[pickedPlayerIndex] = {...allTouches[pickedPlayerIndex], picked: true};
    log(`picked ${pickedPlayer[0].identifier} ${allTouches[pickedPlayerIndex].picked}`); //touch id
  }

  const onTouchStart = (e) => {
    
    
    e.preventDefault();
    const touches = e.changedTouches;
    for (let i = 0; i < touches.length; i++) {
      allTouches.push(copyTouch(touches[i]));
      log(`touch start total: ${allTouches.length - 1}`)
    }
    if(allTouches.length >= 3){
      startTimer();
    }
  }

  const onTouchMove = (e) => {
    e.preventDefault();
    const touches = e.changedTouches;
    for (let i = 0; i < allTouches.length - 1; i++) {
      log(`i ${i}`)
      let touchIndex = touchIndexById(touches[i].identifier);
      if(touchIndex > 0) {
        log(`${touchIndex} from ${allTouches[touchIndex].clientX}, ${allTouches[touchIndex].clientY} to ${touches[i].clientX}, ${touches[i].clientY}`);
        allTouches[touchIndex] = {...allTouches[touchIndex], clientX: touches[i].clientX, clientY:touches[i].clientY};
      } else {
        log("can't find which touch to move");
      }
    }
  }

  const onTouchEnd = (e) => {
    e.preventDefault();
    log("touch end")
    clearInterval(interval);
    timer = 0;
    const touches = e.changedTouches;
    for (let i = 0; i <= allTouches.length - 1; i++) {
      let touchIndex = touchIndexById(touches[i].identifier);
      if(touchIndex >= 0) {
        allTouches.splice(touchIndex, 1);
        log(`removed touch ${touchIndex}`)
      } else {
        log("can't find which touch to end");
      }   
    }
    if(allTouches.length === 1){
      pickedPlayer = null;
    }
  }

  const onTouchCancel = (e) => {
    e.preventDefault();
    log("touch cancel")
    allTouches = [{}];
    clearInterval(interval);
  }

  function onPointerDown(e){
    e.preventDefault();
    // element.setPointerCapture(e.pointerId);
    allTouches.push({identifier: e.pointerId, clientX:e.clientX, clientY:e.clientY})
    log(`pointer down ${e.pointerId} ${allTouches[1].clientX}`)
  }


  function onPointerUp(e){
    e.preventDefault();
    allTouches = allTouches.filter((touch) => touch.identifier !== e.pointerId);
    log(`pointer up ${e.pointerId}`)
  }

  function onPointerMove(e){
    e.preventDefault();
    log(`pointer${e.pointerId} move ${e.clientX}, ${e.clientY}`)

  }

  function log(msg){
    const log = document.getElementById("log");
    log.textContent = `${msg} \n ${log.textContent}`;
    logMsg += `${msg} \n`;
    console.log(msg)
  }

  function copyTouch({ identifier, pageX, pageY, clientX, clientY, screenX, screenY, picked }) {
    return { identifier, pageX, pageY, clientX, clientY, screenX, screenY, picked };
  }

  function touchIndexById(idToFind) {
    for (let i = 0; i< allTouches.length; i++) {
      const id = allTouches[i].identifier;
      if (id === idToFind){
        // log(`index ${i}`);
        return i;
      }
    }
    return -1; //not found
  }



  // function onPointerDown(e) {
  //   console.log('pointer down')
  //   startTimer();

  //   element.setPointerCapture(e.pointerId);
  //   allCapturedPoints = [...allCapturedPoints, e.pointerId]
  //   console.log("all", allCapturedPoints)
  //   capturedPointerId = e.pointerId;
  //   console.log(e.pointerId);
  //   const marker = document.createElement("div");
  //   marker.setAttribute("id", `marker${e.pointerId}`);
  //   marker.setAttribute("style", "width: 100px; height: 100px; color: black;")
  //   marker.setAttribute("style:transform", "translate({x}px, {y}px)");
  //   marker.setAttribute("on:pointerup", `${onPointerUp}`)
  //   marker.setAttribute("on:onpointermove", `${onPointerMove}`)
  //   document.body.append(marker);
  // }

  // function onPointerUp(e) {
  //   capturedPointerId = null;
  //   element.releasePointerCapture(e.pointerId);
  //   allCapturedPoints = [...allCapturedPoints.splice(allCapturedPoints.findIndex(e.pointerId))];
  //   console.log("up all", allCapturedPoints);
  //   if(allCapturedPoints.length === 0){
  //     clearInterval();
  //   }
  // }

  // function onPointerMove(e) {
  //   if (capturedPointerId != e.pointerId)
  //     return;

  //   e.preventDefault();
  //   e.stopPropagation();

  //   const marker = document.getElementById(`marker${e.pointerId}`);
  //   marker.setAttribute("style:transform", `translate(${x}px, ${y}px)`);

  //   x += e.movementX;
  //   y += e.movementY;
  // }
  // function colorForTouch(touch) {
  //   let r = touch.identifier % 16;
  //   let g = Math.floor(touch.identifier / 3) % 16;
  //   let b = Math.floor(touch.identifier / 7) % 16;
  //   r = r.toString(16); // make it a hex digit
  //   g = g.toString(16); // make it a hex digit
  //   b = b.toString(16); // make it a hex digit
  //   const color = `#${r}${g}${b}`;
  //   return color;
  // }
  
</script>

<style>
  .picker-area{
    height: 95%;
    background-color: black;
    color: white;
    overflow-y: scroll;
    overflow-x: auto;
    touch-action: none;
  }
  #log{
    touch-action: none;
  }
</style>

<h1>Player Picker</h1>
<!-- <div bind:this={element}
     on:pointerdown={onPointerDown}
     on:pointerup={onPointerUp}
     on:pointermove={onPointerMove}
     style:transform="translate({x}px, {y}px)"
>
  {capturedPointerId}
-->
<div class="touch-area picker-area"

  on:touchstart={onTouchStart}
  on:touchmove={onTouchMove}
  on:touchend={onTouchEnd}
  on:touchcancel={onTouchCancel}

>
<span style={timer ? "color: red" : "color: white"}>{timer}</span> touch: {allTouches}
  {#each allTouches as touch}
    <svelte:component this={PlayerBubble} touch={touch} allTouches={allTouches} />
  {/each}
  <!-- {#each storePB as touch}
    <svelte:component this={PlayerBubble} touch={touch} />
  {/each} -->
  <pre id="log">loaded</pre>
</div>
<!-- <div class="pointer-area picker-area"

  on:pointerdown={onPointerDown}
  on:pointerup={onPointerUp}
>
pointer: {storePB}

  {#each storePB as touch}
    <svelte:component this={PlayerBubblePointer} touch={touch} />
  {/each}
</div> -->
