<template>
  <div class="photo-gallery">
    <div v-for="(image, index) in images" :key="index" class="photo-card">
      <img :src="image.src" :alt="image.alt" />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      images: [
        { src: '/images/leeze.jpeg', alt: 'Leeze' },
        { src: '/images/gin9.png', alt: 'Gin9' },
      ],
    };
  },
};
</script>

<style scoped>
.photo-gallery {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.photo-card {
  margin: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  overflow: hidden;
  width: calc(50% - 20px); /* 화면의 50% 너비에서 margin 값을 뺀 값 */
}

.photo-card img {
  width: 100%;
  height: auto;
  object-fit: cover;
}

@media (min-width: 768px) {
  .photo-card {
    width: calc(33.333% - 20px); /* 화면이 768px 이상일 때 한 줄에 3장 */
  }
}

@media (min-width: 1024px) {
  .photo-card {
    width: calc(25% - 20px); /* 화면이 1024px 이상일 때 한 줄에 4장 */
  }
}
</style>