<template>
  <div class="world-cup">
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
</template>

<script>
export default {
  emits: ['winner-selected'], // 부모에게 보낼 이벤트를 정의합니다.
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
    };
  },
  computed: {
    currentMatch() {
      const startIndex = this.round * 2;
      return this.candidates.slice(startIndex, startIndex + 2);
    },
    roundTitle() {
      if (this.candidates.length === 4) return '4강';
      if (this.candidates.length === 2) return '결승';
      return '';
    }
  },
  methods: {
    selectCandidate(selected) {
      this.nextRoundCandidates.push(selected);
      
      if (this.nextRoundCandidates.length * 2 === this.candidates.length) {
        // 현재 라운드의 모든 경기가 끝났을 때
        if (this.candidates.length === 2) {
          // 결승전이 끝났으면, 우승자를 부모에게 알립니다.
          this.$emit('winner-selected', this.nextRoundCandidates[0]);
        } else {
          // 다음 라운드로 진출합니다.
          this.candidates = this.nextRoundCandidates;
          this.nextRoundCandidates = [];
          this.round = 0;
        }
      } else {
        // 현재 라운드의 다음 경기로 이동합니다.
        this.round++;
      }
    }
  },
  created() {
    // 컴포넌트가 생성될 때 후보 목록을 무작위로 섞습니다.
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
  align-items: center; /* 이미지가 다른 높이를 가질 수 있으므로 상단 정렬 */
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
  /* height와 object-fit 속성 제거하여 원본 비율 유지 */
  height: auto;
  display: block; /* 이미지 하단 여백 제거 */
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

/* 모바일 화면을 위한 미디어 쿼리 */
@media (max-width: 768px) {
  .world-cup {
    padding: 10px;
  }
  .match-up {
    gap: 10px;
  }
  .vs {
    font-size: 1.5rem;
  }
  h3 {
    font-size: 1rem;
  }
}
</style>