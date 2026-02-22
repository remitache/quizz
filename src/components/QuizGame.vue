<template>
  <div class="quiz-game">
    <div class="quiz-header">
      <div class="progress-bar">
        <div class="progress-fill" :style="{ width: ((currentIndex + 1) / questions.length) * 100 + '%' }"></div>
      </div>
      <div class="quiz-info">
        <span class="question-count">Question {{ currentIndex + 1 }} / {{ questions.length }}</span>
        <span class="score-counter">Score: {{ score }}</span>
      </div>
    </div>

    <div class="question-card">
      <p class="question-text">{{ currentQuestion.question }}</p>
    </div>

    <div class="answers-grid">
      <button
        v-for="(answer, index) in currentQuestion.answers"
        :key="index"
        class="answer-btn"
        :class="{
          correct: answered && index === currentQuestion.correct,
          wrong: answered && index === selectedAnswer && index !== currentQuestion.correct,
          disabled: answered,
        }"
        :disabled="answered"
        @click="selectAnswer(index)"
      >
        {{ answer }}
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  questions: {
    type: Array,
    required: true,
  },
})

const emit = defineEmits(['finish'])

const currentIndex = ref(0)
const score = ref(0)
const selectedAnswer = ref(null)
const answered = ref(false)

const currentQuestion = computed(() => props.questions[currentIndex.value])

function selectAnswer(index) {
  if (answered.value) return
  selectedAnswer.value = index
  answered.value = true

  if (index === currentQuestion.value.correct) {
    score.value++
  }

  setTimeout(() => {
    if (currentIndex.value < props.questions.length - 1) {
      currentIndex.value++
      selectedAnswer.value = null
      answered.value = false
    } else {
      emit('finish', score.value)
    }
  }, 1000)
}
</script>

<style scoped>
.quiz-game {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem 1rem;
  width: 100%;
  max-width: 700px;
  margin: 0 auto;
}

.quiz-header {
  width: 100%;
  margin-bottom: 2rem;
}

.progress-bar {
  width: 100%;
  height: 12px;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 10px;
  overflow: hidden;
  margin-bottom: 0.75rem;
}

.progress-fill {
  height: 100%;
  background: linear-gradient(90deg, #22c55e, #4ade80);
  border-radius: 10px;
  transition: width 0.4s ease;
}

.quiz-info {
  display: flex;
  justify-content: space-between;
  font-family: 'Fredoka', sans-serif;
  font-size: 1.1rem;
  color: rgba(255, 255, 255, 0.9);
  font-weight: 500;
}

.question-card {
  background: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(10px);
  border: 3px solid rgba(255, 255, 255, 0.2);
  border-radius: 24px;
  padding: 2rem 2.5rem;
  margin-bottom: 2rem;
  width: 100%;
  position: relative;
}

.question-card::after {
  content: '';
  position: absolute;
  bottom: -14px;
  left: 40px;
  width: 24px;
  height: 24px;
  background: rgba(255, 255, 255, 0.15);
  border: 3px solid rgba(255, 255, 255, 0.2);
  border-top: none;
  border-left: none;
  transform: rotate(45deg);
}

.question-text {
  font-family: 'Fredoka', sans-serif;
  font-size: 1.4rem;
  font-weight: 600;
  color: #fff;
  line-height: 1.4;
}

.answers-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
  width: 100%;
}

.answer-btn {
  font-family: 'Fredoka', sans-serif;
  font-size: 1.05rem;
  font-weight: 500;
  padding: 1.2rem 1rem;
  background: rgba(255, 255, 255, 0.12);
  backdrop-filter: blur(10px);
  border: 3px solid rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  color: #fff;
  cursor: pointer;
  transition: all 0.25s ease;
}

.answer-btn:hover:not(.disabled) {
  transform: translateY(-3px);
  background: rgba(255, 255, 255, 0.22);
  border-color: rgba(255, 255, 255, 0.5);
}

.answer-btn.correct {
  background: rgba(34, 197, 94, 0.4);
  border-color: #22c55e;
  animation: pop 0.3s ease;
}

.answer-btn.wrong {
  background: rgba(239, 68, 68, 0.4);
  border-color: #ef4444;
  animation: shake 0.4s ease;
}

.answer-btn.disabled {
  cursor: default;
  opacity: 0.7;
}

.answer-btn.disabled.correct {
  opacity: 1;
}

@keyframes pop {
  0% { transform: scale(1); }
  50% { transform: scale(1.05); }
  100% { transform: scale(1); }
}

@keyframes shake {
  0%, 100% { transform: translateX(0); }
  25% { transform: translateX(-6px); }
  75% { transform: translateX(6px); }
}

@media (max-width: 500px) {
  .answers-grid {
    grid-template-columns: 1fr;
  }

  .question-text {
    font-size: 1.2rem;
  }
}
</style>
