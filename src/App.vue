<template>
  <div class="app">
    <ThemeSelect v-if="screen === 'theme'" @select="startQuiz" />
    <QuizGame v-else-if="screen === 'quiz'" :questions="questions" @finish="showScore" @quit="restart" />
    <ScoreBoard v-else :score="finalScore" :total="10" @restart="restart" />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import ThemeSelect from './components/ThemeSelect.vue'
import QuizGame from './components/QuizGame.vue'
import ScoreBoard from './components/ScoreBoard.vue'

import dragonBallData from './data/dragon-ball.json'
import gtoData from './data/gto.json'
import captainTsubasaData from './data/captain-tsubasa.json'

const themeData = {
  'dragon-ball': dragonBallData,
  'gto': gtoData,
  'captain-tsubasa': captainTsubasaData,
}

const screen = ref('theme')
const questions = ref([])
const finalScore = ref(0)

function shuffle(array) {
  const arr = [...array]
  for (let i = arr.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1))
    ;[arr[i], arr[j]] = [arr[j], arr[i]]
  }
  return arr
}

function startQuiz(themeId) {
  const allQuestions = themeData[themeId]
  questions.value = shuffle(allQuestions).slice(0, 10)
  screen.value = 'quiz'
}

function showScore(score) {
  finalScore.value = score
  screen.value = 'score'
}

function restart() {
  screen.value = 'theme'
  questions.value = []
  finalScore.value = 0
}
</script>
