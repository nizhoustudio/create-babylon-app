<script setup lang="ts">
  import { FreeCamera } from "@babylonjs/core/Cameras/freeCamera";
  import { Engine } from "@babylonjs/core/Engines/engine";
  import { HemisphericLight } from "@babylonjs/core/Lights/hemisphericLight";
  import { Vector3 } from "@babylonjs/core/Maths/math.vector";
  import { Scene } from "@babylonjs/core/scene";

  import { ref, onMounted } from 'vue'
  import {MeshBuilder} from "@babylonjs/core";

  const dom = ref()

  onMounted(() => {
    const canvas = dom.value;
    const engine = new Engine(canvas, true);
    const createScene = function () {
      const scene = new Scene(engine);
      const camera = new FreeCamera("camera1",
          new Vector3(0, 5, -10), scene);
      camera.setTarget(Vector3.Zero());
      camera.attachControl(canvas, true);
      const light = new HemisphericLight("light",
          new Vector3(0, 1, 0), scene);
      light.intensity = 0.7;
      const sphere = MeshBuilder.CreateSphere("sphere",
          {diameter: 2, segments: 32}, scene);
      sphere.position.y = 1;
      const ground = MeshBuilder.CreateGround("ground",
          {width: 6, height: 6}, scene);
      return scene;
    };
    const scene = createScene();
    engine.runRenderLoop(function () {
      scene.render();
    });
    window.addEventListener("resize", function () {
      engine.resize();
    });
  })
</script>

<template>
  <canvas id="renderCanvas" ref="dom"></canvas>
</template>

<style scoped>
#renderCanvas {
  width: 100%;
  height: 100%;
  display: block;
  font-size: 0;
}
</style>

