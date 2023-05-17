<script>
  import { writable } from "svelte/store";
  import {
    MeshStandardMaterial,
    SphereGeometry,
    BufferGeometry,
    EllipseCurve,
    LineBasicMaterial,
  } from "three";
  import { DEG2RAD } from "three/src/math/MathUtils";
  import {
    AmbientLight,
    Canvas,
    DirectionalLight,
    Group,
    HemisphereLight,
    Mesh,
    OrbitControls,
    PerspectiveCamera,
    Line,
  } from "threlte";

  let mag = writable(10);

  let curve = new EllipseCurve(
    10,
    0, // ax, aY
    10,
    10, // xRadius, yRadius
    0,
    2 * Math.PI, // aStartAngle, aEndAngle
    false, // aClockwise
    0 // aRotation
  );
  const points = curve.getPoints(50);
  // Create the final object to add to the scene
  // const ellipse = new THREE.Line(
  //   new THREE.BufferGeometry().setFromPoints(points),
  //   new THREE.LineBasicMaterial({ color: 0xff0000 })
  // );

  let charge1 = writable(1.602e-19);
  let charge2 = writable(1.602e-19);
  let posCharge1 = writable(0);
  let posCharge2 = writable(10);
  let k = writable(9e9);
</script>

<div class="main">
  <Canvas>
    <PerspectiveCamera position={{ x: 10, y: 10, z: 10 }} fov={24}>
      <OrbitControls
        maxPolarAngle={DEG2RAD * 180}
        autoRotate={false}
        enableZoom={true}
        target={{ x: 5, y: 0.5 }}
      />
    </PerspectiveCamera>

    <DirectionalLight shadow position={{ x: 3, y: 10, z: 10 }} /> -->
    <DirectionalLight position={{ x: -3, y: 10, z: -10 }} intensity={0.2} />
    <AmbientLight intensity={0.2} color={"#ff0000"} />

    <Group>
      <Mesh
        position={{ x: $posCharge1, y: 0.5 }}
        castShadow
        geometry={new SphereGeometry(1, 64, 32)}
        material={new MeshStandardMaterial({ color: "blue" })}
      />

      <Mesh
        position={{ x: $posCharge2, y: 0.5 }}
        castShadow
        geometry={new SphereGeometry(1, 64, 32)}
        material={new MeshStandardMaterial({ color: "red" })}
      />

      {#each Array(50) as _, i}
        <Line
          interactive
          geometry={new BufferGeometry().setFromPoints(points)}
          material={new LineBasicMaterial({ color: "green" })}
          rotation={{ x: 0, y: i, z: 0 }}
          position={{ x: $posCharge1, y: 0, z: 0 }}
          scale={$mag}
        />
      {/each}

      {#each Array(50) as _, i}
        <Line
          interactive
          geometry={new BufferGeometry().setFromPoints(points)}
          material={new LineBasicMaterial({ color: "green" })}
          rotation={{ x: 0, y: i, z: 0 }}
          position={{ x: $posCharge2, y: 0, z: 0 }}
          scale={$mag}
        />
      {/each}
    </Group>
  </Canvas>
</div>

<div class="wrapper">
  <div class="magValue">
    <input
      type="range"
      name=""
      id=""
      min="0"
      max="10"
      step="1"
      value="5"
      on:input={(e) => {
        $mag = parseInt(e.target.value);
      }}
    />
  </div>

  <input
    type="range"
    name=""
    id=""
    min="0"
    max="50"
    step="1"
    value="0"
    on:input={(e) => {
      $posCharge1 = parseInt(e.target.value);
    }}
  />

  <input
    type="range"
    name=""
    id=""
    min="0"
    max="50"
    step="1"
    value="10"
    on:input={(e) => {
      $posCharge2 = parseInt(e.target.value);
    }}
  />
</div>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  .main {
    height: 90vh;
    width: 100%;
    background: linear-gradient(
      to right,
      #0f2027,
      #203a43,
      #2c5364
    ); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
  }

  .wrapper {
    display: flex;
    flex-direction: column;
    justify-content: center;
    max-width: 300px;
  }
</style>
