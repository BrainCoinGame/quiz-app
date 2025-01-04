<script setup>
import QuizHome from './components/QuizHome.vue'
</script>

<template>
  <div class="app-container">
    
    <QuizHome />
  </div>
</template>

<style scoped>
.app-container {
  width: 100%;
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;
}

.logos {
  display: flex;
  justify-content: center;
  margin-bottom: 2rem;
}

.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>