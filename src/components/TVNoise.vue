<script setup>
import { onMounted, onUnmounted, ref, computed } from 'vue';

const canvasRef = ref(null);
let animationFrameId = null;
let resizeTimeoutId = null;

// Detect if on mobile device for performance optimizations
const isMobile = computed(() => {
  if (typeof window !== 'undefined') {
    return window.innerWidth <= 768;
  }
  return false;
});

onMounted(() => {
  const canvas = canvasRef.value;
  const ctx = canvas.getContext('2d');
  
  // Set canvas size to match its display size
  function resizeCanvas() {
    // Clear any existing timeout
    if (resizeTimeoutId) {
      clearTimeout(resizeTimeoutId);
    }
    
    // Debounce resize for mobile to avoid performance issues
    resizeTimeoutId = setTimeout(() => {
      const { width, height } = canvas.getBoundingClientRect();
      
      // For mobile, use a smaller canvas size for better performance
      const scale = isMobile.value ? 0.75 : 1;
      
      const scaledWidth = Math.floor(width * scale);
      const scaledHeight = Math.floor(height * scale);
      
      if (canvas.width !== scaledWidth || canvas.height !== scaledHeight) {
        canvas.width = scaledWidth;
        canvas.height = scaledHeight;
      }
    }, 100);
  }
  
  resizeCanvas();
  window.addEventListener('resize', resizeCanvas);
  
  // Draw TV static noise
  function drawNoise() {
    const { width, height } = canvas;
    const imageData = ctx.createImageData(width, height);
    const data = imageData.data;
    
    // Use a more efficient pattern for mobile
    const pixelSkip = isMobile.value ? 2 : 1; // Process fewer pixels on mobile
    
    for (let i = 0; i < data.length; i += 4 * pixelSkip) {
      const value = Math.floor(Math.random() * 256);
      
      // Fill current pixel
      data[i] = value; // red
      data[i + 1] = value; // green
      data[i + 2] = value; // blue
      data[i + 3] = 50 + Math.random() * 155; // alpha (semi-transparent)
      
      // If skipping pixels on mobile, copy the same value to adjacent pixels
      if (pixelSkip > 1) {
        for (let j = 1; j < pixelSkip; j++) {
          if (i + j * 4 < data.length) {
            data[i + j * 4] = value;
            data[i + j * 4 + 1] = value;
            data[i + j * 4 + 2] = value;
            data[i + j * 4 + 3] = data[i + 3];
          }
        }
      }
    }
    
    ctx.putImageData(imageData, 0, 0);
    
    // Lower frame rate on mobile for better performance
    const frameDelay = isMobile.value ? 1000/24 : 0; // 24fps on mobile, max on desktop
    
    setTimeout(() => {
      animationFrameId = requestAnimationFrame(drawNoise);
    }, frameDelay);
  }
  
  drawNoise();
});

onUnmounted(() => {
  if (animationFrameId) {
    cancelAnimationFrame(animationFrameId);
  }
  if (resizeTimeoutId) {
    clearTimeout(resizeTimeoutId);
  }
  window.removeEventListener('resize', resizeCanvas);
});
</script>

<template>
  <div class="tv-noise-container" :class="{ 'mobile': isMobile }">
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

/* Mobile optimizations */
.tv-noise-container.mobile .noise-canvas {
  image-rendering: optimizeSpeed; /* Better performance on mobile */
  touch-action: none; /* Prevent unnecessary touch events */
}
</style> 