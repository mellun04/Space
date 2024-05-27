<script>
  import { Canvas } from '@threlte/core'
  import Space from '$lib/components/Space.svelte'
  import '@picocss/pico'

  let weight = "";

  let planet_nr = 4;
  let moving = false

  const planets = [{name:"A neutron star",g:7*10**11}, {name:"Ceres",g:0.028}, {name:"The moon",g:0.17}, {name:"Mars",g:0.38}, {name:"Earth",g:1}, {name:"Uranus",g:0.92}, {name:"Jupiter",g:2.64}, {name:"The sun",g:28}]
  
  let info_tab = false;
  let about_tab = false;

  let tilt = 0

  function wheel(event){
    if(Math.abs(event.deltaX) > 0.5 && !moving){  move(-Math.sign(event.deltaX), true)  }
    if(Math.abs(event.deltaY) > 2){ tilt = event.deltaY/50 }else{ tilt = 0}
  }
  function move(direction, wheel){
    if((direction < 0 && planet_nr > 0)||(direction > 0 && planet_nr < 7)){
      if (!moving){
        planet_nr += direction
        moving = true
        setTimeout(()=>{moving = false}, wheel?1800:1200)
      }
    }
  }
</script>


<div class="nav-bar">
  <button id="drop-down" on:click={()=>{info_tab = !info_tab; about_tab = false}}> Object Info</button>
  <h1 id="top"> {"How much do you weigh on "+planets[planet_nr].name.toLowerCase()+"?"} </h1>
  <button id="drop-down" on:click={()=>{about_tab = !about_tab; info_tab = false}}> About the website </button>
</div>


  <div class="weight">
    <input maxlength=8  type="text" bind:value={weight} id="input" placeholder="Enter weight"/>
    {#if (weight && !isNaN(weight))}  <h2 style="font-size:calc(1vw + 10px);"> {"You would weigh " +Math.round(weight*planets[planet_nr].g*1000)/1000+ " kg!"}</h2>  {/if}
  </div>

  <div class="tab" class:out={!info_tab} id="info-tab">
    <h4 style="margin:0px; color:aliceblue;"> {planets[planet_nr].name} <br> </h4> 
    {#if planet_nr === 0} The most violent and dense object in the known universe thats not a black hole. Neutron stars are formed when a star with a mass 10-25 times the sun dies in a supernova. The core of the star is then compressed by gravity until the nuclei of the atoms literally touch and all electrons and protons are converted into neutrons. You end up with an atomic nucleus the size of a small city but with a mass larger than the sun, and a surface gravity strong enough to bend it's own light around itself. It also rotates stupidly fast, with periods ranging from 30 to 0.001 seconds! {/if}
    {#if planet_nr === 1} This little guy spanning 940 km in diameter was the first known asteroid discovered in 1801. It was announced as a new planet at first but then as an asteroid and now the only dwarf planet inside of Neptune's orbit. It orbits in the asteroid belt between Mars and Jupiter and is the largest object in it. It is mostly made out of ice and rock and it's low gravity would allow a human to jump 17.5 meters!  {/if}
    {#if planet_nr === 2} Earth's only natural satellite and an uncommon sight in the solar system because of all the moon's of the planets our one is the largest compared to the planet. It is tidally looked to earth witch means we only se one side of it. The moon formed when a mars sized planet called Thea hit Earth in it's youth, material was ejected and it snowballed forming into the moon. When it formed it was 10 times closer than it is now, but tidal interactions between the rotating Earth made the moon slowly drift away, which it still does to this day, about 3.8 cm per year. {/if}
    {#if planet_nr === 3} Also known as the red planet, from all the iron oxide (rust) contained on it's surface. Mars is the second smallest planet in the solar system and it has a day of 24.5 hours and a years equal to 1.88 earth years. Mars has two small potato shaped moons called Phobos and Deimos and hosts the largest mountain in the solar system called Olympus Mons with a height of 21.9 km. Even though the Mars of today is a frozen dessert with a thin atmosphere and surface temperatures between -80 and 5 celsius, there are signs of liquid water existing on the surface in the past and where there is water, there is life. {/if}
    {#if planet_nr === 4} The only home we have ever had and the only planet in the universe we know to harbor life. This middle sized rocky planet hosts a plethora of different animals, plants, fungi and prokaryotes including a special species called homo sapiens. With there intellect this species of primates has managed to free themselves from the shackles of space and time and started on the path to discover the cosmos and it laws. This makes them a way for the universe to know itself. {/if}
    {#if planet_nr === 5} The seventh planet from the sun, the smallest gas giant by mass and the coldest planet in the solar system. It and Neptune is classified as ice giants because of the large amount of water ice they contain. Uranus orbits 2.8 billion kilometers from the sun and has an orbital period of 84 years. One weird aspect of it is its axial tilt of 82 degrees that gives it seasonal changes unlike an other planet, the cause of this is unknown. Uranus was not known to the ancients because of its slow orbit and distance from earth and therefore the first planet to be discovered in 1781, although it had been spotted before. {/if}
    {#if planet_nr === 6} The king of the solar system. Jupiter is the fifth planet from the sun and the largest planet in the solar system, more massive than all the other planets combined. This gas giant consists mostly of hydrogen and helium left after the formation of the sun. Despite its huge size jupiter has the fastest rotation rate of all planets in the solar system, completing one rotation every 10 hours. This creates massive coriolis forces driving the biggest storm in the solar system, the great red spot. A storm the size of three Earths that has lasted for more than 400 years. {/if}
    {#if planet_nr === 7} The true ruler of our solar system making up 99.86% of its total mass. It is a G-type main-sequence star called a yellow dwarf made of mostly hydrogen and helium left over from the big bang. In its core the enormous pressure and temperature  generated from the gravity of this enormous ball of plasma lets hydrogen fuse into helium which releases an enormous amount of energy that powers our planet. This radiation pressure fights against the force of gravity and keeps the star in balance, until it runs out of fuel that is. {/if}
  </div>

  <div class="tab" class:out={!about_tab} id="about-tab" >
    <h4 style="margin:0px; color:aliceblue;"> About the website <br> </h4> 
    If you know your physics you know that this site isn't entirely correct because weight and mass is different. Mass is measured in kilograms (kg) and is constant, not dependant on the local gravity. Weight is a force and those are measured in newtons (N) and it depends on the gravity around you. But in this site we use these totally different measures interchangeably. <br>
    The weights on this website is calculated from the surface gravitys on the diffent celestial bodies realative to the surface gravity on Earth (9.82 m/s^2)
  </div>


<div class="bottom-bar">
<div id="scroll-bar" on:wheel={wheel}>

  <!-- svelte-ignore a11y-click-events-have-key-events -->
  <!-- svelte-ignore a11y-no-static-element-interactions -->
  <div id="arrow-left" on:click={()=>move(-1, false)}> </div>

  {#each [0,0,0,0,0,0,0] as planet,i}
    <img class="dot" class:big={(i+1 === planet_nr)} src="dot.png" alt="dot"/>
  {/each}

  <!-- svelte-ignore a11y-click-events-have-key-events -->
  <!-- svelte-ignore a11y-no-static-element-interactions -->
  <div id="arrow-right"   on:click={()=>move(1, false)}> </div>

</div>
</div>

<div id="canvas">
  <Canvas>
    <Space {planet_nr} {tilt} />
  </Canvas>
</div>


<style>
  #top{
    padding:5px;
    color:darkcyan;
    text-overflow:clip;
    white-space:nowrap;
    overflow:hidden;
  }
  #input{
    width:170px;
    margin:15px;
    background-color:rgba(200,200,200,0.5);
    border: none;
    border-radius: 15px;
  }
  ::placeholder {
    color: black;
  }
  #info-tab{
    left:0px;
  }
  #about-tab{
    right:0px;
  }
  .tab{
    width: 400px;
    max-height:70vh;
    background-color: rgba(70, 70, 70, 0.7);
    padding: 10px;
    color: aliceblue;
    overflow: auto;
    position: absolute;
    top:70px;
    transition: transform 1s;
    z-index: -1;
  }
  .tab.out{
    transform: translateY(-80vh);
  }
  #drop-down{
    width: 400px;
    background-color: rgb(70, 70, 70);
    border-color: rgb(52, 52, 52);
    border-radius: 0;
    border-width: 2px;
    font-size: calc(1vw + 5px);
  }
  .nav-bar{
    height: 60px;
    background-color: rgba(70, 70, 70);
    display: flex;
    justify-content: space-between;
  }
  .bottom-bar{
    position: absolute;
    bottom: 20px;
    width: 100vw;
    left:0;
    display: flex;
    justify-content: space-around;
  }
  #scroll-bar{
    width: 20vw;
    height: 50px;
    display: flex;
    justify-content: center;
  }
  .dot{
    padding:15px 3px;
    transition: transform 0.2s;
  }
  .dot.big{
    padding: 10px 3px;
    transform: scale(1.2);
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
  .weight{
    width: 50vw;
    height:80vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-between;
    position: absolute;
    top:60px;
    left:25vw;
  }
  h2{
    color: aliceblue;
    font-size: xx-large;
  }
  h1{
    margin: 5px;
    color:darkcyan;
    font-size: xx-large;
  }
  #canvas{
    background-color: black;
    height: 100%;
    width: 100%;
    position:absolute;
    left: 0px;
    top: 0px;
    z-index: -2;
  }
</style>