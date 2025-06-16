<template>
  <div class="game-container">
    <div class="board">
      <GameCard
        v-for="card in cards"
        :key="card.id"
        :card="card"
        @flip-card="handleCardFlip"
        :disabled="card.disabled || lockBoard"
      />
    </div>

    <GameStats v-if="gameOver" :moves="moves" @restart="restartGame" />
  </div>
</template>

<script>
import GameCard from './components/GameCard.vue'
import GameStats from './components/GameStats.vue'

export default {
  components: { GameCard, GameStats },
  data() {
    return {
      cards: [],
      flippedCards: [],
      moves: 0,
      matchedPairs: 0,
      lockBoard: false,
      gameOver: false,
      icons: ['🍎', '🚗', '🐶', '🌟', '🎵', '⚽', '🧩', '🚀']
    }
  },
  created() {
    this.restartGame()
  },
  methods: {
    generateCards() {
      const pairs = [...this.icons, ...this.icons].map((icon, index) => ({
        id: index + 1,
        icon,
        flipped: false,
        disabled: false
      }))
      return this.shuffle(pairs)
    },
    shuffle(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1))
        ;[array[i], array[j]] = [array[j], array[i]]
      }
      return array
    },
    handleCardFlip(card) {
      if (this.lockBoard || card.flipped || this.flippedCards.length === 2) return
      card.flipped = true
      this.flippedCards.push(card)

      if (this.flippedCards.length === 2) {
        this.moves++
        this.lockBoard = true
        setTimeout(() => {
          this.checkForMatch()
        }, 600)
      }
    },
    checkForMatch() {
      const [card1, card2] = this.flippedCards
      if (card1.icon === card2.icon) {
        card1.disabled = true
        card2.disabled = true
        this.matchedPairs++
        if (this.matchedPairs === this.icons.length) {
          this.gameOver = true
        }
      } else {
        card1.flipped = false
        card2.flipped = false
      }
      this.flippedCards = []
      this.lockBoard = false
    },
    restartGame() {
      this.cards = this.generateCards()
      this.flippedCards = []
      this.moves = 0
      this.matchedPairs = 0
      this.lockBoard = false
      this.gameOver = false
    }
  }
}
</script>

<style scoped>
.game-container {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.board {
  display: grid;
  grid-template-columns: repeat(4, 100px);
  gap: 10px;
  margin: 20px;
}
</style>
