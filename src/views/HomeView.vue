<script setup lang="ts">
import * as THREE from 'three'
import { ref, onMounted } from 'vue'
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js'

// Canvas reference
const canvas = ref<HTMLDivElement | null>(null)

// Create scene
const scene = new THREE.Scene()

// Setup camera
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)
camera.position.z = 5

// const boxGeometry = new THREE.BoxGeometry(1, 1, 1)
// const boxMaterial = new THREE.MeshBasicMaterial({ color: 0xffffff })
// const box = new THREE.Mesh(boxGeometry, boxMaterial)
// scene.add(box)

// const circleGeometry = new THREE.CircleGeometry(1, 1, 1, 1)
// const circleMaterial = new THREE.MeshBasicMaterial({ color: 'red' })
// const circle = new THREE.Mesh(circleGeometry, circleMaterial)
// scene.add(circle)

// Create cube
const cubeGeometry = new THREE.BoxGeometry(2, 2, 2)
const cubeTexture = new THREE.TextureLoader().load(
  '/me/me.jpg'
)
const cubeMaterial = new THREE.MeshBasicMaterial({ map: cubeTexture })
const cube = new THREE.Mesh(cubeGeometry, cubeMaterial)
cube.position.x = -2
scene.add(cube)

// Cube rotation speed
const cubeRotation = {
  x: 0.01,
  y: 0.01
}

// Add lights
const ambientLight = new THREE.AmbientLight(0xffffff)
scene.add(ambientLight)

// Add lights direction
const directionalLight = new THREE.DirectionalLight(0xffffff, 2)
directionalLight.position.set(0, 1, 0)
scene.add(directionalLight)

// Setup renderer
const renderer = new THREE.WebGLRenderer()
renderer.setSize(window.innerWidth, window.innerHeight)

// Animation loop for cube
const animate = () => {
  requestAnimationFrame(animate)
  cube.rotation.x += cubeRotation.x
  cube.rotation.y += cubeRotation.y
  renderer.render(scene, camera)
}

// GLTF Loader
const loader = new GLTFLoader()
let model: any
// Load the model
loader.load('/empire_state_building.glb', function (gltf) {
  model = gltf.scene
  model.scale.set(5, 5, 5)
  model.position.x = 5
  scene.add(model)
  animateModelRotation()
})

// Animation loop for model rotation
const animateModelRotation = () => {
  requestAnimationFrame(animateModelRotation)
  if (model) {
    model.rotation.y += 0.01
    model.rotation.x += 0.001
  }
}

onMounted(() => {
  if (canvas.value) {
    canvas.value.appendChild(renderer.domElement)
  }
  animate()
})
</script>

<template>
  <div ref="canvas" >
    <p class="xxx">
      Lorem ipsum dolor sit amet consectetur adipisicing elit. Dolor iste sit delectus perferendis veniam sint dicta, facilis sapiente cupiditate ducimus eum neque laboriosam odio sed nulla aut possimus! Atque, culpa?
    </p>
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  overflow: hidden;
}
canvas {
  position: relative;
  z-index: -1;
}
.xxx{
  position: absolute;
  z-index: 1000;
  width: 100vw;
  height: 100vh;
  color: #fff;
}
</style>
