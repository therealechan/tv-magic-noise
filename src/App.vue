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
}

body {
  margin: 0;
  padding: 0;
  overflow: hidden;
  color: #ffffff; /* Set default text color */
  background-color: #000000; /* Set a background color in case content doesn't load */
}

#app, #app-container {
  width: 100vw;
  height: 100vh;
  position: fixed;
  top: 0;
  left: 0;
}

/* Hide scrollbars */
::-webkit-scrollbar {
  display: none;
}

* {
  -ms-overflow-style: none;  /* IE and Edge */
  scrollbar-width: none;  /* Firefox */
}
</style>
