<script setup>
import EnhancedTVNoise from './EnhancedTVNoise.vue';
import TVAudio from './TVAudio.vue';
import { onMounted, ref, onUnmounted } from 'vue';
import { Maximize } from 'lucide-vue-next';

// Page metadata - helps for SEO when using Vue Meta or similar solutions
const pageTitle = 'TV Magic Noise - Nostalgic TV Static Experience';
const pageDescription = 'Experience realistic, nostalgic TV static noise in a cozy living room setting. Perfect ambient display for relaxation or decoration.';

// State for UI visibility
const uiVisible = ref(true);
let hideTimeout = null;

// Function to show UI controls
function showUIControls() {
  uiVisible.value = true;
  
  // Clear any existing timeout
  if (hideTimeout) {
    clearTimeout(hideTimeout);
  }
  
  // Set a new timeout to hide UI after 3 seconds
  hideTimeout = setTimeout(() => {
    uiVisible.value = false;
  }, 3000);
}

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
  }, 500);
  
  // Set up event listeners for UI visibility
  document.addEventListener('mousemove', showUIControls);
  document.addEventListener('keydown', showUIControls);
  document.addEventListener('click', showUIControls);
  document.addEventListener('touchstart', showUIControls);
  
  // Initial hide timeout
  showUIControls();
});

// Clean up event listeners on unmount
onUnmounted(() => {
  document.removeEventListener('mousemove', showUIControls);
  document.removeEventListener('keydown', showUIControls);
  document.removeEventListener('click', showUIControls);
  document.removeEventListener('touchstart', showUIControls);
  
  if (hideTimeout) {
    clearTimeout(hideTimeout);
  }
});
</script>

<template>
  <main 
    class="living-room-container" 
    role="main" 
    aria-label="TV Static Living Room Experience"
    @click="showUIControls"
  >
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
    
    <!-- UI Controls container with transition -->
    <div class="ui-controls" :class="{ 'ui-hidden': !uiVisible }">
      <!-- TV Audio Component with toggle button -->
      <TVAudio />
      
      <!-- Fullscreen button in case auto-fullscreen doesn't work -->
      <button 
        @click="toggleFullScreen" 
        class="fullscreen-button"
        aria-label="Enter fullscreen mode"
      >
        <span class="button-icon">
          <Maximize size="16" />
        </span>
        <span class="button-text">Fullscreen</span>
      </button>
    </div>
    
    <!-- Copyright footer with improved semantics -->
    <footer class="copyright-footer" :class="{ 'ui-hidden': !uiVisible }">
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

/* UI Controls container */
.ui-controls {
  position: fixed;
  bottom: 20px;
  right: 20px;
  z-index: 10;
  display: flex;
  flex-direction: column;
  gap: 10px;
  opacity: 1;
  transition: opacity 0.5s ease-in-out;
}

/* Hidden state for UI elements */
.ui-hidden {
  opacity: 0;
  pointer-events: none;
}

.fullscreen-button {
  position: relative;
  padding: 8px 12px;
  background-color: rgba(0, 0, 0, 0.8);
  color: white;
  border: 1px solid rgba(255, 255, 255, 0.3);
  border-radius: 5px;
  cursor: pointer;
  opacity: 0.7;
  transition: opacity 0.3s, background-color 0.3s;
  display: flex;
  align-items: center;
  gap: 5px;
  width: 130px;
  justify-content: center;
}

.fullscreen-button:hover {
  opacity: 1;
  border-color: rgba(255, 255, 255, 0.5);
}

.button-icon {
  display: flex;
  align-items: center;
  justify-content: center;
}

.button-text {
  font-size: 12px;
  font-weight: bold;
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
  transition: opacity 0.5s ease-in-out;
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