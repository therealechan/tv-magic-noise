<script setup>
import LivingRoom from './components/LivingRoom.vue'
import { onMounted } from 'vue'

// Add a page title and description for SEO
document.title = 'TV Magic Noise - Nostalgic TV Static Experience'

// Set up structured data for the application
onMounted(() => {
  // Ensure all metadata is properly loaded
  const meta = document.querySelector('meta[name="description"]')
  if (meta) {
    meta.setAttribute('content', 'Experience realistic, nostalgic TV static noise in a cozy living room setting with authentic TV static sound. Perfect ambient display for relaxation or decoration.')
  }
  
  // Prevent iOS Safari bounce effect
  document.body.addEventListener('touchmove', function(e) {
    if (e.target.tagName.toLowerCase() !== 'input') {
      e.preventDefault();
    }
  }, { passive: false });
  
  // Note: Audio is handled by the TVAudio component in LivingRoom
})
</script>

<template>
  <!-- Root element with descriptive ARIA label -->
  <div id="app-container" role="application" aria-label="TV Magic Noise Application">
    <LivingRoom />
  </div>
</template>

<style>
/* Reset default browser margins and ensure full viewport coverage */
html, body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  font-family: Arial, sans-serif; /* Add a consistent font family */
  -webkit-text-size-adjust: 100%; /* Prevent font scaling in landscape on iOS */
  touch-action: none; /* Prevent default touch actions */
}

body {
  margin: 0;
  padding: 0;
  overflow: hidden;
  color: #ffffff; /* Set default text color */
  background-color: #000000; /* Set a background color in case content doesn't load */
  -webkit-tap-highlight-color: transparent; /* Remove tap highlight on mobile */
}

#app, #app-container {
  width: 100vw;
  height: 100vh;
  position: fixed;
  top: 0;
  left: 0;
}

/* For iOS devices to use full height */
@supports (-webkit-touch-callout: none) {
  #app, #app-container {
    height: -webkit-fill-available;
  }
}

/* Hide scrollbars */
::-webkit-scrollbar {
  display: none;
}

* {
  -ms-overflow-style: none;  /* IE and Edge */
  scrollbar-width: none;  /* Firefox */
  box-sizing: border-box; /* Better box model for mobile */
}

/* Mobile-specific optimizations */
@media (max-width: 768px) {
  * {
    user-select: none; /* Prevent text selection on mobile */
  }
  
  button {
    touch-action: manipulation; /* Optimization for touch events */
  }
}
</style>
