<script>
  import * as THREE from "three";
  import gsap from "gsap";
 


  //Textures
  const textureLoader = new THREE.TextureLoader()
  const texture = textureLoader.load('/color.jpg')
  // Scene
  const scene = new THREE.Scene();

  //Group
  const group = new THREE.Group();
  scene.add(group);


  //Geometry
  const geometry = new THREE.BufferGeometry()

const count = 50
const positionsArray = new Float32Array(count*3*3)
for(let i = 0; i<count*3*3; i++){
  positionsArray[i] = Math.random() -0.5;
}

const positionAttribute = new THREE.BufferAttribute(positionsArray,3)
geometry.setAttribute('position', positionAttribute);
  // const positionsArray= new Float32Array([
  //   0,0,0,
  //   0,1,0,
  //   1,0,0
  // ])

  // const positionAttribute = new THREE.BufferAttribute(positionsArray, 3)

  geometry.setAttribute('position', positionAttribute);


  //Material
  const material = new THREE.MeshBasicMaterial({
    color: 0xff000,
    wireframe: true
  })
  //Mesh
  const mesh = new THREE.Mesh(geometry, material)
  scene.add(mesh);
  // const cube1 = new THREE.Mesh(
  //   new THREE.BoxGeometry(1, 1, 1),
  //   new THREE.MeshBasicMaterial({ color: 0xff0000 })
  // );


  // group.add(cube1);

  // Sizes
  const sizes = {
    width: window.innerWidth,
    height: window.innerHeight,
  };

  //Resizing
  window.addEventListener("resize", () => {
    //Update sizes
    sizes.width = window.innerWidth;
    sizes.height = window.innerHeight;

    //Update camera
    camera.aspect = sizes.width / sizes.height;
    camera.updateProjectionMatrix();

    //Update renderer
    renderer.setSize(sizes.width, sizes.height);
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
  });

  window.addEventListener("dblclick", () => {
    if (!document.fullscreenElement) {
      console.log("dd");
    } else {
      console.log("leave");
    }
  });

  // Camera
  const camera = new THREE.PerspectiveCamera(75, sizes.width / sizes.height);
  camera.translateZ(3);
  scene.add(camera);

  
  // Renderer
  const renderer = new THREE.WebGLRenderer({
    canvas: document.querySelector("canvas.webgl"),
  });
  renderer.setSize(sizes.width, sizes.height);
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
  //Time
  let time = Date.now();
  //Clock
  const clock = new THREE.Clock();


  //Animations
  const tick = () => {
    //Clock
    const elapsedTime = clock.getElapsedTime();
    
    window.requestAnimationFrame(tick);
    // cube1.rotation.y = Math.sin(elapsedTime);
   
    //Renderer
    renderer.render(scene, camera);
  };
  tick();

   // const cube2 = new THREE.Mesh(
  //   new THREE.BoxGeometry(1, 1, 1),
  //   new THREE.MeshBasicMaterial({ color: 0x00ff00 })
  // );

  // const cube3 = new THREE.Mesh(
  //   new THREE.BoxGeometry(1, 1, 1),
  //   new THREE.MeshBasicMaterial({ color: 0x0000ff })
  // );

  // cube2.translateX(-2);
  // cube3.translateX(2);

// const aspectRation = sizes.width / sizes.height;
  // const camera = new THREE.OrthographicCamera(-1 * aspectRation, 1 * aspectRation, 1, -1, 0.1, 100);
  // camera.position.x = 2;
  // camera.position.z = 2;
  // camera.position.y = 2;
  // camera.lookAt(cube1.position);
  // scene.add(camera);

  // gsap.to(cube1.rotation, { duration: 1, delay:1, y: 3 });

//times
    // const currentTime = Date.now();
    // const deltaTime = currentTime - time;
    // time = currentTime;

 // cube1.position.x = Math.cos(elapsedTime);
    // camera.lookAt(cube1.position);

</script>

<main />

<style>
</style>
