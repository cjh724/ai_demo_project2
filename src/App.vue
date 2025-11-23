<script setup>
import { ref, computed } from 'vue';
import PhotoGallery from './components/PhotoGallery.vue';

const winner = ref(null);
const gameKey = ref(0);

// winner 상태에 따라 동적으로 CSS 클래스를 변경합니다.
const containerClass = computed(() => {
  return winner.value ? 'container final-layout' : 'container initial-layout';
});

// 자식 컴포넌트에서 우승자가 선택되었을 때 호출될 함수입니다.
function handleWinnerSelected(selectedWinner) {
  winner.value = selectedWinner;
}

// 게임을 다시 시작하는 함수입니다.
function restartGame() {
  winner.value = null;
  gameKey.value++; // key를 변경하여 PhotoGallery 컴포넌트를 강제로 다시 렌더링합니다.
}
</script>

<template>
  <header>
    <h1>진구 이상형 월드컵</h1>
  </header>

  <main :class="containerClass">
    <div class="main-photo">
      <img src="/images/gin9.png" alt="Main Photo" />
    </div>

    <!-- 최종 우승 시 하트 아이콘 표시 -->
    <div v-if="winner" class="heart-container">
      <span>❤️</span>
    </div>

    <div class="right-panel">
      <!-- 우승자가 없으면 토너먼트를 보여줍니다. -->
      <PhotoGallery v-if="!winner" :key="gameKey" @winner-selected="handleWinnerSelected" />

      <!-- 우승자가 있으면 우승자 정보를 보여줍니다. -->
      <div v-if="winner" class="winner-display">
        <h2>최종 이상형</h2>
        <img :src="winner.src" :alt="winner.alt">
        <h3>{{ winner.alt }}</h3>
        <button @click="restartGame">다시 하기</button>
      </div>
    </div>
  </main>
</template>

<style>
/* 전역 스타일 */
body {
  background-color: #f0f0f0;
}
#app {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
header {
  text-align: center;
  margin-bottom: 2rem;
}
h1 {
  font-size: 2.5rem;
  color: #333;
}

/* 레이아웃 스타일 */
.container {
  display: flex;
  align-items: center;
}

/* 초기 레이아웃: gin9 상단, 토너먼트 하단 */
.initial-layout {
  flex-direction: column;
}
.initial-layout .main-photo {
  width: 100%;
  max-width: 400px;
  margin-bottom: 20px;
}
.initial-layout .right-panel {
  width: 100%;
}

/* 최종 레이아웃: gin9 좌측, 하트, 우승자 우측 */
.final-layout {
  flex-direction: row;
  justify-content: center;
  gap: 15px;
}
.final-layout .main-photo,
.final-layout .right-panel {
  flex: 1; /* 공간을 유연하게 차지하도록 설정 */
  min-width: 0; /* flex item이 작아질 수 있도록 설정 */
}

.main-photo img {
  width: 100%;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
  display: block; /* 이미지 하단 여백 제거 */
}

/* 하트 아이콘 스타일 */
.heart-container {
  flex-shrink: 0; /* 하트 아이콘은 작아지지 않도록 설정 */
  text-align: center;
  font-size: 4rem;
  color: red;
  animation: beat 0.7s infinite alternate;
}
@keyframes beat {
  to { transform: scale(1.2); }
}

/* 우승자 표시 스타일 */
.winner-display {
  text-align: center;
  background-color: #fff;
  padding: 15px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
}
.winner-display h2 {
  color: #28a745;
}
.winner-display img {
  width: 100%;
  border-radius: 10px;
  border: 5px solid #28a745;
  margin: 10px 0;
  display: block;
}
.winner-display button {
  margin-top: 15px;
  padding: 10px 20px;
  font-size: 1rem;
  cursor: pointer;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
}

/* 모바일 화면을 위한 미디어 쿼리 */
@media (max-width: 768px) {
  #app {
    padding: 10px;
  }
  h1 {
    font-size: 1.8rem;
  }
  .final-layout {
    gap: 10px; /* 모바일에서 간격 줄이기 */
  }
  .heart-container {
    font-size: 2rem; /* 모바일에서 하트 크기 줄이기 */
  }
  .winner-display {
    padding: 10px;
  }
  .winner-display h2, .winner-display h3 {
    font-size: 1rem;
  }
  .winner-display button {
    font-size: 0.9rem;
    padding: 8px 12px;
  }
}
</style>