<script>
  import { T, useTask } from '@threlte/core'
  import { TextureLoader, BackSide } from 'three'
  import { useLoader } from '@threlte/core'
  import { lerp } from 'three/src/math/MathUtils.js';

  let n = 0

  
  //function texture_load(url){
  //  return fetch_data = new Promise(resolve => {
  //    const data = useLoader(TextureLoader).load(url)
  //    if ( data ) { resolve ( data ) ; n++;}
  //  }) ;
  //}

  //texture_load('/earth.jpg').then( result => {const earth = result})
  

  const earth = useLoader(TextureLoader).load('earth.jpg')
  const jupiter = useLoader(TextureLoader).load('jupiter.jpg')
  const sun = useLoader(TextureLoader).load('sun.jpg')
  const moon = useLoader(TextureLoader).load('moon.jpg')
  const ceres = useLoader(TextureLoader).load('ceres.jpg')
  const neutron = useLoader(TextureLoader).load('blue_star.jpg')
  const mars = useLoader(TextureLoader).load('mars.jpg')
  const uranus = useLoader(TextureLoader).load('uranus.jpg')


  const planet_rad = [0.00157, 1/13, 0.273, 0.532, 1, 4, 11,109]
  const planet_pos = [0, 0.2, 0.8, 2.5, 6, 14, 40, 200]
  const planet_rot = [2000,1,1,1,1,1,1,1]

  function pos(index) {
    let n = Math.floor(index)
    if(Number.isInteger(index)){ return planet_pos[n]  }else{
      return lerp(planet_pos[n], planet_pos[n+1], index-n) }
  }
  
  const stars = useLoader(TextureLoader).load('/stars.jpg')
  
  export let planet_nr = 0;
  let cam_x = 4;
  let cam_z = 4;
  
  let rotation = 0
  useTask((delta) => {
    rotation += delta/2
    if(Math.abs(planet_nr-cam_x) > delta/2){
      cam_x += Math.sign(planet_nr-cam_x)*delta
    }else{   cam_x = planet_nr  }
    
    if(cam_x <= planet_nr){  cam_z *= (planet_rad[planet_nr]/cam_z*4)**(delta*4)  }
    if(cam_x >= planet_nr && cam_x-planet_nr < 0.5){  cam_z *= (planet_rad[planet_nr]/cam_z*4)**(delta)  }

  })
</script>


<T.PerspectiveCamera
makeDefault
position={[pos(cam_x), 0, cam_z]} args = {[50,2, 0.001, 100000]}
on:create={({ ref }) => {  ref.lookAt(pos(cam_x), 0, 0)  }} />

<T.DirectionalLight position={[0, 0, 30]}/>


{#each planet_rad as rad, i}

  <T.Mesh
  position = {[planet_pos[i],0,0]}
  rotation.y={rotation*planet_rot[i]}>
    <T.SphereGeometry args={[rad,128,64]}/>
    
    {#if i === 0 && $neutron} <T.MeshBasicMaterial map={$neutron} /> {/if}
    {#if i === 1 && $ceres} <T.MeshStandardMaterial map={$ceres} /> {/if}
    {#if i === 2 && $moon} <T.MeshStandardMaterial map={$moon} /> {/if}
    {#if i === 3 && $mars} <T.MeshStandardMaterial map={$mars} /> {/if}
    {#if i === 4 && $earth} <T.MeshStandardMaterial map={$earth} /> {/if}
    {#if i === 5 && $uranus} <T.MeshStandardMaterial map={$uranus} /> {/if}
    {#if i === 6 && $jupiter} <T.MeshStandardMaterial map={$jupiter} /> {/if}
    {#if i === 7 && $sun} <T.MeshBasicMaterial map={$sun} /> {/if}
    
  </T.Mesh>

{/each}

<T.Mesh
position = {[0,0,0]}
scale = {50000}>
  <T.SphereGeometry/>
  {#if $stars} <T.MeshBasicMaterial map={$stars} side={BackSide}/> {/if}
</T.Mesh>