<script setup>
import EnhancedTVNoise from './EnhancedTVNoise.vue';
import { onMounted, ref } from 'vue';

// Page metadata - helps for SEO when using Vue Meta or similar solutions
const pageTitle = 'TV Magic Noise - Nostalgic TV Static Experience';
const pageDescription = 'Experience realistic, nostalgic TV static noise in a cozy living room setting. Perfect ambient display for relaxation or decoration.';

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
    
    // Update document title dynamically for better SEO
    document.title = pageTitle;
    // You could add meta description here if using vue-meta or similar
  }, 500);
});
</script>

<template>
  <main class="living-room-container" role="main" aria-label="TV Static Living Room Experience">
    <!-- Noise layer that fills the entire screen -->
    <section class="noise-background" aria-hidden="true">
      <EnhancedTVNoise />
    </section>
    
    <!-- Living room image with transparent TV screen on top -->
    <img 
      src="/living-room.png" 
      alt="Living Room with TV showing static noise" 
      class="room-image"
      loading="eager"
      fetchpriority="high"
    />
    
    <!-- Fullscreen button in case auto-fullscreen doesn't work -->
    <button 
      @click="toggleFullScreen" 
      class="fullscreen-button"
      aria-label="Enter fullscreen mode"
    >
      Fullscreen
    </button>
    
    <!-- Copyright footer with improved semantics -->
    <footer class="copyright-footer">
      <p>Created by <a href="https://www.0xechan.xyz" target="_blank" rel="noopener noreferrer">0xechan</a></p>
    </footer>
  </main>
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

/* Copyright footer styles */
.copyright-footer {
  position: fixed;
  bottom: 10px;
  left: 0;
  width: 100%;
  text-align: center;
  z-index: 5;
  font-family: Arial, sans-serif;
  font-size: 12px;
}

.copyright-footer p {
  color: rgba(255, 255, 255, 0.3);
  margin: 0;
}

.copyright-footer a {
  color: rgba(255, 255, 255, 0.4);
  text-decoration: none;
  transition: color 0.3s;
}

.copyright-footer a:hover {
  color: rgba(255, 255, 255, 0.7);
}
</style> 