<script setup>
import { onMounted, onUnmounted, ref } from 'vue';
import * as THREE from 'three';

const containerRef = ref(null);
let scene, camera, renderer, material, clock;
let animationFrameId;

onMounted(() => {
  initThreeJS();
  animate();
  
  window.addEventListener('resize', onResize);
  
  // Initial resize to ensure it fits properly
  setTimeout(onResize, 100);
});

onUnmounted(() => {
  if (animationFrameId) {
    cancelAnimationFrame(animationFrameId);
  }
  
  window.removeEventListener('resize', onResize);
  
  if (renderer) {
    renderer.dispose();
  }
  
  if (material) {
    material.dispose();
  }
});

function initThreeJS() {
  const container = containerRef.value;
  
  // Scene setup
  scene = new THREE.Scene();
  camera = new THREE.OrthographicCamera(-0.5, 0.5, 0.5, -0.5, 0, 1);
  
  // Renderer setup
  renderer = new THREE.WebGLRenderer({ 
    alpha: true,
    antialias: true
  });
  
  // Set initial size - will be resized properly in onResize()
  renderer.setSize(window.innerWidth, window.innerHeight);
  container.appendChild(renderer.domElement);
  
  // Create TV static material with custom shader
  const vertexShader = `
    varying vec2 vUv;
    void main() {
      vUv = uv;
      gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
    }
  `;
  
  const fragmentShader = `
    uniform float time;
    varying vec2 vUv;
    
    float random(vec2 co) {
      return fract(sin(dot(co.xy, vec2(12.9898, 78.233))) * 43758.5453);
    }
    
    void main() {
      // Basic static
      float r = random(vUv + time);
      
      // Create TV scan lines
      float scanLine = sin(vUv.y * 100.0) * 0.1 + 0.9;
      
      // Flickering effect
      float flicker = sin(time * 10.0) * 0.05 + 0.95;
      
      // Edge darkening for old TV look
      float vignetteAmount = 0.5;
      float vignette = mix(1.0, 1.0 - vignetteAmount, length(vUv - 0.5) * 1.5);
      
      // Color tint for old TV
      vec3 color = vec3(r * scanLine * flicker * vignette);
      color.b *= 0.8; // Slight blue reduction for vintage look
      
      gl_FragColor = vec4(color, 0.9);
    }
  `;
  
  // Create material with shader
  material = new THREE.ShaderMaterial({
    uniforms: {
      time: { value: 0.0 }
    },
    vertexShader,
    fragmentShader,
    transparent: true
  });
  
  // Create a simple plane to cover viewport
  const geometry = new THREE.PlaneGeometry(1, 1);
  const mesh = new THREE.Mesh(geometry, material);
  scene.add(mesh);
  
  // Clock for animation timing
  clock = new THREE.Clock();
}

function animate() {
  animationFrameId = requestAnimationFrame(animate);
  
  if (material && clock) {
    material.uniforms.time.value = clock.getElapsedTime();
  }
  
  if (renderer && scene && camera) {
    renderer.render(scene, camera);
  }
}

function onResize() {
  if (!containerRef.value || !renderer) return;
  
  // Get the exact dimensions of the container
  const { width, height } = containerRef.value.getBoundingClientRect();
  
  // Update renderer size to match
  renderer.setSize(width, height, false);
}
</script>

<template>
  <div ref="containerRef" class="tv-noise-container"></div>
</template>

<style scoped>
.tv-noise-container {
  width: 100%;
  height: 100%;
  overflow: hidden;
  position: relative;
  display: block;
}

canvas {
  display: block;
  width: 100%;
  height: 100%;
}
</style> 