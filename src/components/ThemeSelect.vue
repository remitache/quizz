<template>
  <div class="theme-select">
    <h1 class="title">Anime Quiz</h1>
    <p class="subtitle">Choose your theme and test your knowledge!</p>
    <div class="theme-grid">
      <button
        v-for="theme in themes"
        :key="theme.id"
        class="theme-card"
        :style="{ '--accent': theme.color }"
        @click="$emit('select', theme.id)"
      >
        <span class="theme-emoji">{{ theme.emoji }}</span>
        <span class="theme-name">{{ theme.name }}</span>
        <span class="theme-desc">{{ theme.description }}</span>
      </button>
    </div>
  </div>
</template>

<script setup>
defineEmits(['select'])

const themes = [
  {
    id: 'dragon-ball',
    name: 'Dragon Ball',
    description: '20 questions about Saiyans, Namekians & more',
    emoji: '🐉',
    color: '#f59e0b',
  },
  {
    id: 'gto',
    name: 'GTO',
    description: '20 questions about the Great Teacher Onizuka',
    emoji: '👊',
    color: '#ef4444',
  },
  {
    id: 'captain-tsubasa',
    name: 'Captain Tsubasa',
    description: '20 questions about the legendary soccer anime',
    emoji: '⚽',
    color: '#22c55e',
  },
]
</script>

<style scoped>
.theme-select {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem 1rem;
}

.title {
  font-family: 'Bangers', cursive;
  font-size: 4rem;
  color: #fff;
  text-shadow: 4px 4px 0 rgba(0, 0, 0, 0.3);
  margin-bottom: 0.25rem;
  letter-spacing: 3px;
}

.subtitle {
  font-family: 'Fredoka', sans-serif;
  font-size: 1.2rem;
  color: rgba(255, 255, 255, 0.85);
  margin-bottom: 2.5rem;
}

.theme-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
  gap: 1.5rem;
  width: 100%;
  max-width: 800px;
}

.theme-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  padding: 2rem 1.5rem;
  background: rgba(255, 255, 255, 0.12);
  backdrop-filter: blur(10px);
  border: 3px solid rgba(255, 255, 255, 0.2);
  border-radius: 20px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-family: 'Fredoka', sans-serif;
}

.theme-card:hover {
  transform: translateY(-6px) scale(1.03);
  background: rgba(255, 255, 255, 0.2);
  border-color: var(--accent);
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.25);
}

.theme-emoji {
  font-size: 3rem;
}

.theme-name {
  font-size: 1.6rem;
  font-weight: 700;
  color: #fff;
}

.theme-desc {
  font-size: 0.9rem;
  color: rgba(255, 255, 255, 0.7);
  text-align: center;
}

@media (max-width: 500px) {
  .theme-select {
    padding: 1rem 0.5rem;
  }

  .title {
    font-size: 2.8rem;
  }

  .subtitle {
    font-size: 1rem;
    text-align: center;
    margin-bottom: 1.5rem;
  }

  .theme-grid {
    grid-template-columns: 1fr;
    gap: 1rem;
  }

  .theme-card {
    padding: 1.5rem 1rem;
    flex-direction: row;
    gap: 1rem;
  }

  .theme-emoji {
    font-size: 2.4rem;
  }

  .theme-name {
    font-size: 1.3rem;
  }

  .theme-desc {
    text-align: left;
    font-size: 0.85rem;
  }

  .theme-card > :not(.theme-emoji) {
    text-align: left;
  }
}
</style>
