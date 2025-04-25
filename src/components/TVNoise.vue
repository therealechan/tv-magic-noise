<script setup>
import { onMounted, onUnmounted, ref } from 'vue';

const canvasRef = ref(null);
let animationFrameId = null;

onMounted(() => {
  const canvas = canvasRef.value;
  const ctx = canvas.getContext('2d');
  
  // Set canvas size to match its display size
  function resizeCanvas() {
    const { width, height } = canvas.getBoundingClientRect();
    if (canvas.width !== width || canvas.height !== height) {
      canvas.width = width;
      canvas.height = height;
    }
  }
  
  resizeCanvas();
  window.addEventListener('resize', resizeCanvas);
  
  // Draw TV static noise
  function drawNoise() {
    const { width, height } = canvas;
    const imageData = ctx.createImageData(width, height);
    const data = imageData.data;
    
    for (let i = 0; i < data.length; i += 4) {
      const value = Math.floor(Math.random() * 256);
      data[i] = value; // red
      data[i + 1] = value; // green
      data[i + 2] = value; // blue
      data[i + 3] = 50 + Math.random() * 155; // alpha (semi-transparent)
    }
    
    ctx.putImageData(imageData, 0, 0);
    animationFrameId = requestAnimationFrame(drawNoise);
  }
  
  drawNoise();
});

onUnmounted(() => {
  if (animationFrameId) {
    cancelAnimationFrame(animationFrameId);
  }
  window.removeEventListener('resize', resizeCanvas);
});
</script>

<template>
  <div class="tv-noise-container">
    <canvas ref="canvasRef" class="noise-canvas"></canvas>
  </div>
</template>

<style scoped>
.tv-noise-container {
  width: 100%;
  height: 100%;
  overflow: hidden;
  position: relative;
}

.noise-canvas {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
}
</style> 