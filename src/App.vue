<script setup>
import PhotoGallery from './components/PhotoGallery.vue'
</script>

<template>
  <header>
    <h1>진구 이상형 월드컵</h1>
  </header>
  <main class="container">
    <div class="main-photo">
      <img src="/images/gin9.png" alt="Main Photo" />
    </div>
    <div class="world-cup-container">
      <PhotoGallery />
    </div>
  </main>
</template>

<style>
/* Global styles */
body {
  background-color: #f0f0f0;
}

#app {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

header {
  text-align: center;
  margin-bottom: 2rem;
}

h1 {
  font-size: 2.5rem;
  color: #333;
}

/* Layout */
.container {
  display: flex;
  gap: 20px;
}

.main-photo {
  flex-basis: 30%; /* 왼쪽 메인 사진이 차지할 너비 */
}

.main-photo img {
  width: 100%;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
}

.world-cup-container {
  flex-basis: 70%; /* 오른쪽 월드컵 컨텐츠가 차지할 너비 */
}
</style>