<template>
  <div class="world-cup">
    <div v-if="!winner">
      <h2>{{ roundTitle }}</h2>
      <div class="match-up">
        <div class="candidate" @click="selectCandidate(currentMatch[0])">
          <img :src="currentMatch[0].src" :alt="currentMatch[0].alt">
          <h3>{{ currentMatch[0].alt }}</h3>
        </div>
        <div class="vs">VS</div>
        <div class="candidate" @click="selectCandidate(currentMatch[1])">
          <img :src="currentMatch[1].src" :alt="currentMatch[1].alt">
          <h3>{{ currentMatch[1].alt }}</h3>
        </div>
      </div>
    </div>
    <div v-else class="winner-container">
      <h2>최종 우승!</h2>
      <div class="winner">
        <img :src="winner.src" :alt="winner.alt">
        <h3>{{ winner.alt }}</h3>
      </div>
      <button @click="restart">다시 시작</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      candidates: [
        { src: '/images/suzi.jpeg', alt: '수지' },
        { src: '/images/leeze.jpeg', alt: '리즈' },
        { src: '/images/minji.jpeg', alt: '민지' },
        { src: '/images/karina.jpeg', alt: '카리나' },
      ],
      round: 0,
      nextRoundCandidates: [],
      winner: null,
    };
  },
  computed: {
    currentMatch() {
      const startIndex = this.round * 2;
      return this.candidates.slice(startIndex, startIndex + 2);
    },
    roundTitle() {
      if (this.candidates.length === 4) {
        return '4강';
      }
      if (this.candidates.length === 2) {
        return '결승';
      }
      return '';
    }
  },
  methods: {
    selectCandidate(selected) {
      this.nextRoundCandidates.push(selected);
      
      if (this.nextRoundCandidates.length * 2 === this.candidates.length) {
        // 현재 라운드 종료
        if (this.candidates.length === 2) {
          // 결승전 종료, 우승자 결정
          this.winner = this.nextRoundCandidates[0];
        } else {
          // 다음 라운드로 진출
          this.candidates = this.nextRoundCandidates;
          this.nextRoundCandidates = [];
          this.round = 0;
        }
      } else {
        // 다음 매치로 이동
        this.round++;
      }
    },
    restart() {
      this.candidates = [
        { src: '/images/suzi.jpeg', alt: '수지' },
        { src: '/images/leeze.jpeg', alt: '리즈' },
        { src: '/images/minji.jpeg', alt: '민지' },
        { src: '/images/karina.jpeg', alt: '카리나' },
      ].sort(() => Math.random() - 0.5); // 순서 섞기
      this.round = 0;
      this.nextRoundCandidates = [];
      this.winner = null;
    }
  },
  created() {
    // 컴포넌트 생성 시 후보 목록 순서 섞기
    this.candidates.sort(() => Math.random() - 0.5);
  }
};
</script>

<style scoped>
.world-cup {
  background-color: #fff;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
  text-align: center;
}

h2 {
  margin-bottom: 20px;
}

.match-up {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 20px;
}

.candidate {
  cursor: pointer;
  transition: transform 0.2s;
  width: 45%;
}

.candidate:hover {
  transform: scale(1.05);
}

.candidate img {
  width: 100%;
  height: 400px; /* 이미지 높이 수정 */
  object-fit: cover; /* 비율 유지하며 채우기 */
  border-radius: 8px;
  border: 3px solid transparent;
}

.candidate:hover img {
  border-color: #007bff;
}

.vs {
  font-size: 2rem;
  font-weight: bold;
  color: #dc3545;
}

.winner-container {
  text-align: center;
}

.winner img {
  width: 70%;
  max-width: 400px;
  border-radius: 10px;
  border: 5px solid #28a745;
}

button {
  margin-top: 20px;
  padding: 10px 20px;
  font-size: 1rem;
  cursor: pointer;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
}
</style>