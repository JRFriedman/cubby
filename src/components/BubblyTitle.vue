<template>
  <div class="bubbly-title">
    <span v-for="(letter, index) in letters" :key="index" class="bubbly-letter">{{ letter }}</span>
  </div>
</template>

<script>
export default {
  name: 'BubblyTitle',
  props: {
    text: {
      type: String,
      default: 'Cubby'
    }
  },
  computed: {
    letters() {
      return this.text.split('');
    }
  }
}
</script>

<style scoped>
.bubbly-title {
  display: flex;
  justify-content: center;
  margin-bottom: 1rem;
}

.bubbly-letter {
  font-size: 3rem;
  font-weight: bold;
  color: #4a4a4a;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
  animation: bubble 2s ease-in-out infinite alternate;
  margin: 0 0.1em;
}

@keyframes bubble {
  0% {
    transform: scale(1);
  }
  100% {
    transform: scale(1.1);
  }
}

/* Stagger the animation for each letter */
.bubbly-letter:nth-child(1) { animation-delay: 0s; }
.bubbly-letter:nth-child(2) { animation-delay: 0.1s; }
.bubbly-letter:nth-child(3) { animation-delay: 0.2s; }
.bubbly-letter:nth-child(4) { animation-delay: 0.3s; }
.bubbly-letter:nth-child(5) { animation-delay: 0.4s; }

@media (max-width: 768px) {
  .bubbly-letter {
    font-size: 2.5rem;
  }
}
</style>
