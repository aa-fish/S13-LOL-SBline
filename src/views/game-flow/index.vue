<template>
  <div class="game-flow">
    <div class="game-flow-container" ref="gameFlowRef"></div>
  </div>
</template>

<script lang="ts" setup>
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
import { ref, onMounted } from "vue";

const gameFlowRef = ref<HTMLElement | null>(null);

function init() {
  let container = gameFlowRef.value;
  let width = container.offsetWidth;
  let height = container.offsetHeight;

  // 场景
  let scene = new THREE.Scene();
//   scene.background = new THREE.Color(0xf1f1f1);

  let gridHelper = new THREE.GridHelper(3000, 50, 0xffffff, 0xffffff);
  scene.add(gridHelper);

  let camera = new THREE.PerspectiveCamera(70, width / height, 0.001, 99999);
  camera.position.set(600, 1660, 660);
  camera.lookAt(0, 0, 0);

  const renderer = new THREE.WebGLRenderer();
  renderer.setSize(width, height);
  renderer.render(scene, camera);
  container.appendChild(renderer.domElement);
  function render() {
    renderer.render(scene, camera);
  }

  const controls = new OrbitControls(camera, renderer.domElement);
  controls.addEventListener("change", function () {
    renderer.render(scene, camera);
  });

  window.addEventListener("resize", (e) => {
    if (container) {
      // updated size
      width = container.offsetWidth;
      height = container.offsetHeight;

      // updated camera
      camera.aspect = width / height;
      camera.updateProjectionMatrix();

      // updated renderer
      renderer.setSize(width, height);
      renderer.render(scene, camera);
    }
  });
}

onMounted(() => {
  init();
});
</script>

<style scoped>
.game-flow {
  width: 100%;
  height: 100vh;
}
.game-flow-container {
  height: 100%;
}
</style>
