<script setup>
import EnhancedTVNoise from './EnhancedTVNoise.vue';
import { onMounted } from 'vue';

// Function to toggle fullscreen mode
function toggleFullScreen() {
  if (!document.fullscreenElement) {
    document.documentElement.requestFullscreen().catch(err => {
      console.error(`Error attempting to enable fullscreen: ${err.message}`);
    });
  }
}

// Auto enter fullscreen on mount
onMounted(() => {
  // Small delay to ensure everything is loaded
  setTimeout(() => {
    toggleFullScreen();
  }, 500);
});
</script>

<template>
  <div class="living-room-container">
    <!-- Noise layer that fills the entire screen -->
    <div class="noise-background">
      <EnhancedTVNoise />
    </div>
    <!-- Living room image with transparent TV screen on top -->
    <img src="/living-room.png" alt="Living Room" class="room-image" />
    
    <!-- Fullscreen button in case auto-fullscreen doesn't work -->
    <button @click="toggleFullScreen" class="fullscreen-button">
      Fullscreen
    </button>
  </div>
</template>

<style scoped>
.living-room-container {
  position: relative;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}

.noise-background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
}

.room-image {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover; /* Changed from contain to cover to ensure full width display */
  z-index: 2; /* Higher z-index to be on top of the noise */
}

.fullscreen-button {
  position: fixed;
  bottom: 20px;
  right: 20px;
  z-index: 10;
  padding: 10px 15px;
  background-color: rgba(0, 0, 0, 0.7);
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  opacity: 0.5;
  transition: opacity 0.3s;
}

.fullscreen-button:hover {
  opacity: 1;
}
</style> 