<template>
  <div class="quiz-game">
    <div class="quiz-header">
      <div class="header-top">
        <span class="question-count">Question {{ currentIndex + 1 }} / {{ shuffledQuestions.length }}</span>
        <button class="quit-btn" @click="showQuitConfirm = true">Quit</button>
      </div>
      <div class="progress-bar">
        <div class="progress-fill" :style="{ width: ((currentIndex + 1) / shuffledQuestions.length) * 100 + '%' }"></div>
      </div>
      <div class="score-counter">Score: {{ score }} / {{ currentIndex + (answered ? 1 : 0) }}</div>
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

    <!-- Quit confirmation modal -->
    <div v-if="showQuitConfirm" class="modal-overlay" @click.self="showQuitConfirm = false">
      <div class="modal-card">
        <p class="modal-text">Are you sure you want to quit?</p>
        <p class="modal-subtext">Your progress will be lost.</p>
        <div class="modal-buttons">
          <button class="modal-btn modal-btn-cancel" @click="showQuitConfirm = false">Continue Playing</button>
          <button class="modal-btn modal-btn-confirm" @click="$emit('quit')">Yes, Quit</button>
        </div>
      </div>
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

const emit = defineEmits(['finish', 'quit'])

function shuffleAnswers(questions) {
  return questions.map((q) => {
    const correctAnswer = q.answers[q.correct]
    const shuffled = [...q.answers]
    for (let i = shuffled.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1))
      ;[shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]]
    }
    return {
      question: q.question,
      answers: shuffled,
      correct: shuffled.indexOf(correctAnswer),
    }
  })
}

const shuffledQuestions = ref(shuffleAnswers(props.questions))
const currentIndex = ref(0)
const score = ref(0)
const selectedAnswer = ref(null)
const answered = ref(false)
const showQuitConfirm = ref(false)

const currentQuestion = computed(() => shuffledQuestions.value[currentIndex.value])

function selectAnswer(index) {
  if (answered.value) return
  selectedAnswer.value = index
  answered.value = true

  if (index === currentQuestion.value.correct) {
    score.value++
  }

  setTimeout(() => {
    if (currentIndex.value < shuffledQuestions.value.length - 1) {
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

.header-top {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 0.75rem;
}

.question-count {
  font-family: 'Fredoka', sans-serif;
  font-size: 1.1rem;
  color: rgba(255, 255, 255, 0.9);
  font-weight: 500;
}

.quit-btn {
  font-family: 'Fredoka', sans-serif;
  font-size: 0.9rem;
  font-weight: 600;
  padding: 0.4rem 1.2rem;
  background: rgba(239, 68, 68, 0.25);
  border: 2px solid rgba(239, 68, 68, 0.5);
  border-radius: 10px;
  color: #fca5a5;
  cursor: pointer;
  transition: all 0.25s ease;
}

.quit-btn:hover {
  background: rgba(239, 68, 68, 0.4);
  border-color: #ef4444;
  color: #fff;
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

.score-counter {
  font-family: 'Fredoka', sans-serif;
  font-size: 1.1rem;
  color: rgba(255, 255, 255, 0.9);
  font-weight: 500;
  text-align: right;
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

/* Quit confirmation modal */
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.6);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 100;
  animation: fadeIn 0.2s ease;
}

.modal-card {
  background: linear-gradient(135deg, #4c1d95, #6d28d9);
  border: 3px solid rgba(255, 255, 255, 0.2);
  border-radius: 24px;
  padding: 2.5rem;
  text-align: center;
  max-width: 400px;
  width: 90%;
  animation: slideUp 0.3s ease;
}

.modal-text {
  font-family: 'Fredoka', sans-serif;
  font-size: 1.4rem;
  font-weight: 600;
  color: #fff;
  margin-bottom: 0.5rem;
}

.modal-subtext {
  font-family: 'Fredoka', sans-serif;
  font-size: 1rem;
  color: rgba(255, 255, 255, 0.6);
  margin-bottom: 2rem;
}

.modal-buttons {
  display: flex;
  gap: 1rem;
  justify-content: center;
}

.modal-btn {
  font-family: 'Fredoka', sans-serif;
  font-size: 1rem;
  font-weight: 600;
  padding: 0.8rem 1.5rem;
  border-radius: 14px;
  cursor: pointer;
  transition: all 0.25s ease;
}

.modal-btn-cancel {
  background: rgba(255, 255, 255, 0.15);
  border: 2px solid rgba(255, 255, 255, 0.3);
  color: #fff;
}

.modal-btn-cancel:hover {
  background: rgba(255, 255, 255, 0.25);
}

.modal-btn-confirm {
  background: rgba(239, 68, 68, 0.3);
  border: 2px solid rgba(239, 68, 68, 0.6);
  color: #fca5a5;
}

.modal-btn-confirm:hover {
  background: rgba(239, 68, 68, 0.5);
  color: #fff;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
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
  .quiz-game {
    padding: 1rem 0.5rem;
  }

  .question-count {
    font-size: 0.95rem;
  }

  .score-counter {
    font-size: 0.95rem;
  }

  .question-card {
    padding: 1.5rem;
    border-radius: 18px;
  }

  .question-card::after {
    bottom: -12px;
    left: 28px;
    width: 20px;
    height: 20px;
  }

  .question-text {
    font-size: 1.1rem;
  }

  .answers-grid {
    grid-template-columns: 1fr;
    gap: 0.75rem;
  }

  .answer-btn {
    font-size: 1rem;
    padding: 1rem;
    border-radius: 14px;
  }

  .modal-card {
    padding: 2rem 1.5rem;
  }

  .modal-text {
    font-size: 1.2rem;
  }

  .modal-buttons {
    flex-direction: column;
  }

  .modal-btn {
    width: 100%;
  }
}
</style>
