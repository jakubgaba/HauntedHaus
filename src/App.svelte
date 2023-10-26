<script>
    import * as THREE from "three";
    import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";
    import GUI from "lil-gui/dist/lil-gui.esm.min";

    THREE.ColorManagement.enabled = false;

    /**
     * Base
     */
    // Debug
    const gui = new GUI();
    // Canvas
    const canvas = document.querySelector("canvas.webgl");

    // Scene
    const scene = new THREE.Scene();

    //Fog
    const fog = new THREE.Fog("#262837", 1, 15);
    scene.fog = fog;
    /**
     * Textures
     */
    const textureLoader = new THREE.TextureLoader();

    const doorColorTexture = textureLoader.load("/door/color.jpg");
    const doorAlphaTexture = textureLoader.load("/door/alpha.jpg");
    const doorAmbientOcclusionTexture = textureLoader.load(
        "/door/ambientOcclusion.jpg"
    );
    const doorHeightTexture = textureLoader.load("/door/height.jpg");
    const doorNormalTexture = textureLoader.load("/door/normal.jpg");
    const doorMetalnessTexture = textureLoader.load("/door/metalness.jpg");
    const doorRoughnessTexture = textureLoader.load("/door/roughness.jpg");

    const bricksColorTexture = textureLoader.load("/bricks/color.jpg");
    const bricksAmbientOcclusionTexture = textureLoader.load(
        "/bricks/ambientOcclusion.jpg"
    );
    const bricksNormalTexture = textureLoader.load("/bricks/normal.jpg");
    const bricksRoughnessTexture = textureLoader.load("/bricks/roughness.jpg");

    const grassColorTexture = textureLoader.load("/grass/color.jpg");
    const grassAmbientOcclusionTexture = textureLoader.load(
        "/grass/ambientOcclusion.jpg"
    );
    const grassNormalTexture = textureLoader.load("/grass/normal.jpg");
    const grassRoughnessTexture = textureLoader.load("/grass/roughness.jpg");

    grassColorTexture.repeat.set(8, 8);
    grassAmbientOcclusionTexture.repeat.set(8, 8);
    grassNormalTexture.repeat.set(8, 8);
    grassRoughnessTexture.repeat.set(8, 8);

    grassColorTexture.wrapS = THREE.RepeatWrapping;
    grassAmbientOcclusionTexture.wrapS = THREE.RepeatWrapping;
    grassNormalTexture.wrapS = THREE.RepeatWrapping;
    grassRoughnessTexture.wrapS = THREE.RepeatWrapping;

    grassColorTexture.wrapT = THREE.RepeatWrapping;
    grassAmbientOcclusionTexture.wrapT = THREE.RepeatWrapping;
    grassNormalTexture.wrapT = THREE.RepeatWrapping;
    grassRoughnessTexture.wrapT = THREE.RepeatWrapping;

    /**
     * House
     */
    const house = new THREE.Group();
    scene.add(house);

    //Walls
    const walls = new THREE.Mesh(
        new THREE.BoxGeometry(6, 2.5, 6),
        new THREE.MeshStandardMaterial({
            map: bricksColorTexture,
            aoMap: bricksAmbientOcclusionTexture,
            normalMap: bricksNormalTexture,
            roughnessMap: bricksRoughnessTexture,
        })
    );
    walls.position.y = 2.5 / 2;
    scene.add(walls);

    // Floor
    const floor = new THREE.Mesh(
        new THREE.PlaneGeometry(30, 30),
        new THREE.MeshStandardMaterial({
            map: grassColorTexture,
            aoMap: grassAmbientOcclusionTexture,
            normalMap: grassNormalTexture,
            roughnessMap: grassRoughnessTexture,
        })
    );
    floor.rotation.x = -Math.PI * 0.5;
    floor.position.y = 0;
    house.add(floor);

    //Roof
    const roof = new THREE.Mesh(
        new THREE.ConeGeometry(5, 1, 4),
        new THREE.MeshStandardMaterial({ color: "#b35f45" })
    );
    roof.position.y = 2.5 + 0.5;
    roof.rotation.y = Math.PI / 4;
    house.add(roof);

    //Door
    const door = new THREE.Mesh(
        new THREE.PlaneGeometry(2.2, 2.2, 100, 100),
        new THREE.MeshStandardMaterial({
            map: doorColorTexture,
            transparent: true,
            alphaMap: doorAlphaTexture,
            displacementMap: doorHeightTexture,
            aoMap: doorAmbientOcclusionTexture,
            displacementScale: 0.1,
            normalMap: doorNormalTexture,
            metalnessMap: doorMetalnessTexture,
            roughnessMap: doorRoughnessTexture,
        })
    );
    door.position.z = 3 + 0.01;
    door.position.y = 1;
    house.add(door);

    //Bush
    const bushGeometry = new THREE.SphereGeometry(1, 16, 16);
    const bushMaterial = new THREE.MeshStandardMaterial({ color: "#89c854" });

    const bush1 = new THREE.Mesh(bushGeometry, bushMaterial);
    bush1.scale.set(0.5, 0.5, 0.5);
    bush1.position.set(0.8, 0.2, 3);

    const bush2 = new THREE.Mesh(bushGeometry, bushMaterial);
    bush2.scale.set(0.3, 0.3, 0.3);
    bush2.position.set(0.8 * 1.75, 0.1, 3);

    const bush3 = new THREE.Mesh(bushGeometry, bushMaterial);
    bush3.scale.set(0.5, 0.5, 0.5);
    bush3.position.set(-0.8, 0.2, 3);

    const bush4 = new THREE.Mesh(bushGeometry, bushMaterial);
    bush4.scale.set(0.3, 0.3, 0.3);
    bush4.position.set(-0.8 * 1.75, 0.2, 3);

    const bush5 = new THREE.Mesh(bushGeometry, bushMaterial);
    bush5.scale.set(0.25, 0.25, 0.25);
    bush5.position.set(-0.8 * 1.75, 0.15, 3.5);
    house.add(bush5, bush1, bush2, bush3, bush4);

    //Graves
    const graves = new THREE.Group();
    scene.add(graves);

    const graveGeometry = new THREE.BoxGeometry(0.6, 0.8, 0.2);
    const graveMaterial = new THREE.MeshStandardMaterial({ color: "grey" });

    for (let i = 0; i < 50; i++) {
        const angle = Math.random() * Math.PI * 2;
        const radius = 6 + Math.random() * 8;
        const x = Math.sin(angle) * radius;
        const z = Math.cos(angle) * radius;

        const grave = new THREE.Mesh(graveGeometry, graveMaterial);
        grave.position.set(x, 0.3, z);
        grave.rotation.y = Math.random() * 0.5;
        grave.rotation.z = Math.random() * 0.2;
        graves.add(grave);
    }

    /**
     * Lights
     */
    // Ambient light
    const ambientLight = new THREE.AmbientLight("#ffffff", 0.3);
    gui.add(ambientLight, "intensity").min(0).max(1).step(0.001);
    scene.add(ambientLight);

    // Directional light
    const moonLight = new THREE.DirectionalLight("#b9d5ff", 0.1);
    moonLight.position.set(4, 5, -2);
    gui.add(moonLight, "intensity").min(0).max(3).step(0.001);
    gui.add(moonLight.position, "x").min(-5).max(5).step(0.001);
    gui.add(moonLight.position, "y").min(-5).max(5).step(0.001);
    gui.add(moonLight.position, "z").min(-5).max(5).step(0.001);
    scene.add(moonLight);

    //Door light
    const doorLight = new THREE.PointLight("#ff7d46", 1, 15);
    doorLight.position.set(0, 2.15, 3.5);

    house.add(doorLight);

    /**
     * Sizes
     */
    const sizes = {
        width: window.innerWidth,
        height: window.innerHeight,
    };

    window.addEventListener("resize", () => {
        // Update sizes
        sizes.width = window.innerWidth;
        sizes.height = window.innerHeight;

        // Update camera
        camera.aspect = sizes.width / sizes.height;
        camera.updateProjectionMatrix();

        // Update renderer
        renderer.setSize(sizes.width, sizes.height);
        renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
    });

    /**
     * Camera
     */
    // Base camera
    const camera = new THREE.PerspectiveCamera(
        75,
        sizes.width / sizes.height,
        0.1,
        100
    );
    camera.position.x = 4;
    camera.position.y = 2;
    camera.position.z = 5;
    scene.add(camera);

    // Controls
    // @ts-ignore
    const controls = new OrbitControls(camera, canvas);
    controls.enableDamping = true;

    /**
     * Renderer
     */
    const renderer = new THREE.WebGLRenderer({
        canvas: canvas,
    });
    renderer.outputColorSpace = THREE.LinearSRGBColorSpace;
    renderer.setSize(sizes.width, sizes.height);
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
    renderer.setClearColor("#262837");
    /**
     * Animate
     */
    const clock = new THREE.Clock();

    const tick = () => {
        const elapsedTime = clock.getElapsedTime();

        // Update controls
        controls.update();

        // Render
        renderer.render(scene, camera);

        // Call tick again on the next frame
        window.requestAnimationFrame(tick);
    };

    tick();
</script>

<main />

<style>
</style>
