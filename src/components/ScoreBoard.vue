<template>
  <div class="score-board">
    <div class="score-card">
      <span class="trophy">{{ trophy }}</span>
      <h2 class="score-title">{{ message }}</h2>
      <div class="score-display">
        <span class="score-number">{{ score }}</span>
        <span class="score-total">/ {{ total }}</span>
      </div>
      <p class="score-subtitle">correct answers</p>
      <button class="play-again-btn" @click="$emit('restart')">Play Again</button>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  score: {
    type: Number,
    required: true,
  },
  total: {
    type: Number,
    default: 10,
  },
})

defineEmits(['restart'])

const trophy = computed(() => {
  const ratio = props.score / props.total
  if (ratio === 1) return '👑'
  if (ratio >= 0.8) return '🏆'
  if (ratio >= 0.6) return '🎉'
  if (ratio >= 0.4) return '💪'
  return '📚'
})

const message = computed(() => {
  const ratio = props.score / props.total
  if (ratio === 1) return 'Perfect Score!'
  if (ratio >= 0.8) return 'Amazing!'
  if (ratio >= 0.6) return 'Well Done!'
  if (ratio >= 0.4) return 'Not Bad!'
  return 'Keep Studying!'
})
</script>

<style scoped>
.score-board {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 2rem 1rem;
  min-height: 60vh;
}

.score-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  background: rgba(255, 255, 255, 0.12);
  backdrop-filter: blur(10px);
  border: 3px solid rgba(255, 255, 255, 0.2);
  border-radius: 28px;
  padding: 3rem 4rem;
  animation: slideUp 0.5s ease;
}

.trophy {
  font-size: 4rem;
  margin-bottom: 0.5rem;
}

.score-title {
  font-family: 'Bangers', cursive;
  font-size: 2.5rem;
  color: #fff;
  text-shadow: 3px 3px 0 rgba(0, 0, 0, 0.2);
  letter-spacing: 2px;
}

.score-display {
  display: flex;
  align-items: baseline;
  gap: 0.25rem;
  margin: 0.5rem 0;
}

.score-number {
  font-family: 'Bangers', cursive;
  font-size: 5rem;
  color: #4ade80;
  text-shadow: 3px 3px 0 rgba(0, 0, 0, 0.2);
}

.score-total {
  font-family: 'Fredoka', sans-serif;
  font-size: 2rem;
  color: rgba(255, 255, 255, 0.7);
  font-weight: 600;
}

.score-subtitle {
  font-family: 'Fredoka', sans-serif;
  font-size: 1.1rem;
  color: rgba(255, 255, 255, 0.7);
  margin-bottom: 1.5rem;
}

.play-again-btn {
  font-family: 'Fredoka', sans-serif;
  font-size: 1.2rem;
  font-weight: 600;
  padding: 1rem 3rem;
  background: linear-gradient(135deg, #8b5cf6, #6366f1);
  border: none;
  border-radius: 16px;
  color: #fff;
  cursor: pointer;
  transition: all 0.25s ease;
  box-shadow: 0 6px 20px rgba(99, 102, 241, 0.4);
}

.play-again-btn:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 25px rgba(99, 102, 241, 0.5);
}

@keyframes slideUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@media (max-width: 500px) {
  .score-board {
    padding: 1.5rem 0.5rem;
    min-height: 50vh;
  }

  .score-card {
    padding: 2rem 1.5rem;
    border-radius: 22px;
    width: 100%;
  }

  .trophy {
    font-size: 3rem;
  }

  .score-title {
    font-size: 1.8rem;
  }

  .score-number {
    font-size: 3.5rem;
  }

  .score-total {
    font-size: 1.5rem;
  }

  .play-again-btn {
    width: 100%;
    padding: 1rem 2rem;
  }
}
</style>
