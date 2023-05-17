<script>
  import { MeshStandardMaterial, SphereGeometry } from "three";
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
  } from "threlte";
</script>

<Canvas>
  <PerspectiveCamera position={{ x: 10, y: 10, z: 10 }} fov={24}>
    <OrbitControls
      maxPolarAngle={DEG2RAD * 180}
      autoRotate={false}
      enableZoom={true}
      target={{ x: 5, y: 0.5 }}
    />
  </PerspectiveCamera>

  <!-- <DirectionalLight shadow position={{ x: 3, y: 10, z: 10 }} /> -->
  <!-- <DirectionalLight position={{ x: -3, y: 10, z: -10 }} intensity={0.2} /> -->
  <!-- <AmbientLight intensity={0.2} color={"#ff0000"} /> -->

  <!-- Cube -->
  <Group scale={$scale}>
    <Mesh
      position={{ y: 0.5 }}
      castShadow
      geometry={new SphereGeometry(1, 64, 32)}
      material={new MeshStandardMaterial({ color: "#333333" })}
    />

    <Mesh
      position={{ x: 10, y: 0.5 }}
      castShadow
      geometry={new SphereGeometry(1, 64, 32)}
      material={new MeshStandardMaterial({ color: "#333333" })}
    />
  </Group>

  <!-- Floor -->
  <!-- <Mesh
      receiveShadow
      rotation={{ x: -90 * (Math.PI / 180) }}
      geometry={new CircleBufferGeometry(3, 72)}
      material={new MeshStandardMaterial({ side: DoubleSide, color: "white" })}
    /> -->
</Canvas>
