<script setup>
import { ref, computed } from 'vue';
import PhotoGallery from './components/PhotoGallery.vue';

const winner = ref(null);
const gameKey = ref(0);

// 우승자 여부에 따라 동적으로 클래스를 부여합니다.
const containerClass = computed(() => {
  return winner.value ? 'container final-layout' : 'container initial-layout';
});

function handleWinnerSelected(selectedWinner) {
  winner.value = selectedWinner;
}

function restartGame() {
  winner.value = null;
  gameKey.value++; 
}
</script>

<template>
  <header>
    <h1>진구 이상형 월드컵</h1>
  </header>

  <main :class="containerClass">
    <!-- 왼쪽 영역 (gin9 사진) -->
    <div class="photo-wrapper">
      <img src="/images/gin9.png" alt="Main Photo" class="main-photo" />
    </div>

    <!-- 오른쪽 영역 (토너먼트 또는 우승자) -->
    <div class="content-wrapper">
      <!-- 토너먼트 UI -->
      <PhotoGallery
        v-if="!winner"
        :key="gameKey"
        @winner-selected="handleWinnerSelected"
      />

      <!-- 우승자 UI -->
      <div v-else class="winner-container">
        <div class="heart-icon">❤️</div>
        <div class="winner-display">
          <h2>최종 이상형</h2>
          <img :src="winner.src" :alt="winner.alt" />
          <h3>{{ winner.alt }}</h3>
          <button @click="restartGame">다시 하기</button>
        </div>
      </div>
    </div>
  </main>
</template>

<style>
/* 전역 스타일 */
body { background-color: #f0f0f0; }
#app {
  max-width: 1200px;
  margin: 0 auto;
  padding: 10px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
header { text-align: center; margin-bottom: 1.5rem; }
h1 { font-size: 2rem; color: #333; }

/* 레이아웃 컨테이너 */
.container {
  display: flex;
  gap: 15px;
  align-items: flex-start;
}
.photo-wrapper, .content-wrapper {
  flex: 1;
  min-width: 0;
}
.main-photo {
  width: 100%;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
  display: block;
}

/* === 상태별 레이아웃 === */

/* 1. 토너먼트 진행 시 (initial-layout) */
/* 데스크탑: 기본값인 좌우 배치(row) 사용 */

/* 2. 최종 우승자 결정 시 (final-layout) */
/* 데스크탑 & 모바일: 항상 좌우 배치(row) 유지 */
.final-layout {
  align-items: center; /* 세로 중앙 정렬 */
}

/* === 우승자 UI 스타일 === */
.winner-container {
  display: flex;
  align-items: center;
  gap: 15px;
}
.heart-icon {
  font-size: 2.5rem;
  color: red;
  animation: beat 0.7s infinite alternate;
}
@keyframes beat { to { transform: scale(1.2); } }

.winner-display {
  flex: 1;
  text-align: center;
  background-color: #fff;
  padding: 15px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
}
.winner-display h2 { font-size: 1.2rem; color: #28a745; margin: 0 0 10px; }
.winner-display h3 { font-size: 1.1rem; margin: 10px 0 15px; }
.winner-display img {
  width: 100%;
  border-radius: 10px;
  border: 4px solid #28a745;
}
.winner-display button {
  margin-top: 15px;
  padding: 8px 15px;
  font-size: 0.9rem;
  cursor: pointer;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
}

/* === 모바일 반응형 === */
@media (max-width: 768px) {
  /* 토너먼트일 때만 상하 배치로 변경 */
  .initial-layout {
    flex-direction: column;
    align-items: center;
  }
  .initial-layout .photo-wrapper {
    width: 100%;
    max-width: 400px;
  }
  .initial-layout .content-wrapper {
    width: 100%;
  }

  /* 최종 우승자일 때는 좌우 배치 유지하면서 크기만 조정 */
  .final-layout {
    gap: 10px;
  }
  .heart-icon {
    font-size: 1.5rem;
  }
  .winner-display { padding: 8px; }
  .winner-display h2 { font-size: 1rem; }
  .winner-display h3 { font-size: 0.9rem; margin: 5px 0 10px; }
  .winner-display button { padding: 6px 10px; font-size: 0.8rem; }
}
</style>