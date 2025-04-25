<script setup>
import { ref, onMounted, watch, computed } from 'vue';
import { Volume2, VolumeX } from 'lucide-vue-next';

// State to track audio status
const isPlaying = ref(false); // Start with false to handle autoplay restrictions
const audioElement = ref(null);
const hasInteracted = ref(false);

// Detect if on mobile device
const isMobile = computed(() => {
  if (typeof window !== 'undefined') {
    return window.innerWidth <= 768;
  }
  return false;
});

// Function to toggle audio on/off
function toggleAudio() {
  hasInteracted.value = true;
  
  if (audioElement.value) {
    if (isPlaying.value) {
      audioElement.value.pause();
    } else {
      // Play and handle any errors
      const playPromise = audioElement.value.play();
      
      if (playPromise !== undefined) {
        playPromise.catch(error => {
          console.error("Audio playback failed:", error);
          isPlaying.value = false;
        });
      }
    }
    isPlaying.value = !isPlaying.value;
  }
}

// Watch for audio element readiness
watch(audioElement, (newAudio) => {
  if (newAudio) {
    // Set audio properties
    newAudio.loop = true;
    newAudio.volume = 0.3; // Start at 30% volume for better user experience
    
    // Try to play if user has already interacted
    if (hasInteracted.value && !isPlaying.value) {
      toggleAudio();
    }
  }
}, { immediate: false });

// Set up audio when component mounts
onMounted(() => {
  // Most browsers block autoplay until user interacts with the page
  // Listen for user interaction to enable audio
  const userInteractionEvents = ['click', 'touchstart', 'keypress'];
  
  const enableAudio = () => {
    if (!hasInteracted.value && audioElement.value && !isPlaying.value) {
      hasInteracted.value = true;
      toggleAudio();
    }
    
    // Remove event listeners after first interaction
    userInteractionEvents.forEach(event => {
      document.removeEventListener(event, enableAudio);
    });
  };
  
  // Add event listeners to enable audio on first interaction
  userInteractionEvents.forEach(event => {
    document.addEventListener(event, enableAudio, { once: true });
  });
});
</script>

<template>
  <div class="tv-audio-container" :class="{ 'mobile': isMobile }">
    <!-- Hidden audio element -->
    <audio ref="audioElement" src="/audio/whitenoise-34872.mp3" preload="auto"></audio>
    
    <!-- Toggle button -->
    <button 
      @click="toggleAudio" 
      class="audio-toggle-button"
      :class="{ 'audio-off': !isPlaying }"
      :aria-label="isPlaying ? 'Turn off TV audio' : 'Turn on TV audio'"
    >
      <span class="button-icon">
        <Volume2 v-if="isPlaying" :size="isMobile ? 18 : 16" />
        <VolumeX v-else :size="isMobile ? 18 : 16" />
      </span>
      <span class="button-text">{{ isPlaying ? 'Sound ON' : 'Sound OFF' }}</span>
    </button>
  </div>
</template>

<style scoped>
.tv-audio-container {
  position: relative;
  z-index: 10;
}

.audio-toggle-button {
  display: flex;
  align-items: center;
  gap: 5px;
  padding: 8px 12px;
  background-color: rgba(0, 0, 0, 0.8); /* Slightly more opaque for better visibility */
  color: white;
  border: 1px solid rgba(255, 255, 255, 0.3); /* Add a subtle border */
  border-radius: 5px;
  cursor: pointer;
  opacity: 0.7; /* Increased base opacity for better visibility */
  transition: opacity 0.3s, background-color 0.3s;
  width: 130px;
  justify-content: center;
}

.audio-toggle-button:hover {
  opacity: 1;
  border-color: rgba(255, 255, 255, 0.5);
}

.audio-toggle-button.audio-off {
  background-color: rgba(80, 0, 0, 0.8);
}

.button-icon {
  display: flex;
  align-items: center;
  justify-content: center;
}

.button-text {
  font-size: 12px;
  font-weight: bold; /* Make text bolder for readability */
}

/* Mobile optimizations */
.tv-audio-container.mobile .audio-toggle-button {
  padding: 10px 15px; /* Larger touch target */
  width: auto;
  min-width: 120px;
}

.tv-audio-container.mobile .button-text {
  font-size: 14px; /* Larger text for better readability */
}

/* For small mobile screens */
@media (max-width: 480px) {
  .tv-audio-container.mobile .audio-toggle-button {
    min-width: 110px;
  }
}

/* Active state for mobile - improve touch feedback */
.tv-audio-container.mobile .audio-toggle-button:active {
  transform: scale(0.98);
  opacity: 1;
}
</style> 