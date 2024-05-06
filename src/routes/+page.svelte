<script>
  import { Canvas } from '@threlte/core'
  import Space from '$lib/components/Space.svelte'
  import '@picocss/pico'

  let weight = "";

  let planet_nr = 4;
  let moving = false

  const planets = [{name:"A neutron star",g:7*10**11}, {name:"Ceres",g:0.028}, {name:"The moon",g:0.17}, {name:"Mars",g:0.38}, {name:"Earth",g:1}, {name:"Uranus",g:0.92}, {name:"Jupiter",g:2.64}, {name:"The sun",g:28}]
  
  let info_tab = false;
</script>


<div class="nav-bar">
  <h1 style="padding:5px; color:darkcyan"> Gravity</h1>
  <button id="drop-down" on:click={()=>{info_tab = !info_tab}}> Info</button>
</div>

<div style="display:flex; justify-content:space-between;">

  <div class="header">
    <h2> {"How much do you weigh on "+planets[planet_nr].name.toLowerCase()+"?"}</h2>
  
    <input maxlength=8  type="text" bind:value={weight} placeholder="Enter weight (kg)" style="width:200px"/>
  
    {#if (weight && !isNaN(weight))}  <h2> {"You would weigh " +Math.round(weight*planets[planet_nr].g*1000)/1000+ " kg!"}</h2>  {/if}
    {#if (!weight || isNaN(weight))}  <h2> You would weigh ___ kg! </h2>  {/if}
  </div>

  <div id="info-tab" style={info_tab?"display:block":"display:none"}>
    <h4 style="margin:0px; color:aliceblue;"> {planets[planet_nr].name} <br> </h4> 
    {#if planet_nr === 4} The only home we have ever had and the only planet in the universe we know to harbor life. As far as we know it is unique. It hosts a plethora of differnet animals, plants, fungi and prokaryotes including a special species called homo sapiens. With there intelect this species of primates has managed to free themselves from the shakles of space and time and started on the path to descover the cosmos and it laws. This makes them a way for the universe to know itself. {/if}
  </div>

</div>


<div class="scroll-bar">

  <!-- svelte-ignore a11y-click-events-have-key-events -->
  <!-- svelte-ignore a11y-no-static-element-interactions -->
  <div id="arrow-left" on:click={()=>{
    if (!moving && planet_nr > 0){
      planet_nr--
      moving = true
      setTimeout(()=>{moving = false}, 1200)
    } }}> </div>

  {#each [0,0,0,0,0,0,0] as planet,i}
    <img style={"padding:"+((i+1 === planet_nr)?"10px":"15px")+" 3px"} src="dot.png" alt="dot"/>
  {/each}

  <!-- svelte-ignore a11y-click-events-have-key-events -->
  <!-- svelte-ignore a11y-no-static-element-interactions -->
  <div id="arrow-right" style="background-image: url('arrow-left.png');"   on:click={()=>{
    if (!moving && planet_nr < 7){
      planet_nr++
      moving = true
      setTimeout(()=>{moving = false}, 1200)
    } }}> </div>

</div>

<div id="canvas">
  <Canvas>
  <Space {planet_nr} />
  </Canvas>
</div>


<style>
  #info-tab{
    width: 400px;
    max-height:75vh;
    background-color: rgba(70, 70, 70, 0.7);
    padding: 10px;
    color: aliceblue;
    overflow: auto;
  }
  #drop-down{
    width: 400px;
    background-color: rgb(70, 70, 70);
    border-color: black;
    border-radius: 0;
    border-width: 2px;
  }
  .nav-bar{
    height: 60px;
    background-color: rgba(70, 70, 70);
    display: flex;
    justify-content: space-between;
  }
  .scroll-bar{
    position: absolute;
    bottom: 20px;
    width: 20vw;
    left:40vw;
    height: 50px;
    display: flex;
    justify-content: space-between;
  }
  #arrow-left{
    right:40vw;
    min-width: 50px;
    min-height: 50px;
    background-image: url("/arrow-left.png");
    background-size: contain;
    transition: transform 0.1s;
  }
  #arrow-left:hover{
    transform: scale(1.1);
  }
  #arrow-right{
    right:40vw;
    min-width: 50px;
    min-height: 50px;
    background-image: url("/arrow-right.png");
    background-size: contain;
    transition: transform 0.1s;
  }
  #arrow-right:hover{
    transform: scale(1.1);
  }

  .header{
    margin-left: 20px;
    margin-top: 20vh;
    height: 50vh;
    width: 50vw;
    display: flex;
    flex-direction: column;
    justify-content: space-evenly;
  }
  h2{
    color: aliceblue;
    font-size: xx-large;
  }
  #canvas{
    background-color: black;
    height: 100%;
    width: 100%;
    position:absolute;
    left: 0px;
    top: 0px;
    z-index: -1;
  }
</style>