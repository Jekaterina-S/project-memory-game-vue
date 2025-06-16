<template>
  <div
    class="card"
    :class="{ flipped: card.flipped }"
    @click="flipCard"
  >
    <div class="card-inner">
      <div class="card-front">?</div>
      <div class="card-back">{{ card.icon }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'GameCard',
  props: {
    card: Object,
    disabled: Boolean
  },
  methods: {
    flipCard() {
      if (this.disabled || this.card.flipped) return;
      this.$emit('flip-card', this.card);
    }
  }
}
</script>

<style scoped>
.card {
  width: 100px;
  height: 100px;
  perspective: 1000px;
  cursor: pointer;
}

.card-inner {
  width: 100%;
  height: 100%;
  transition: transform 0.6s;
  transform-style: preserve-3d;
  position: relative;
}

.card.flipped .card-inner {
  transform: rotateY(180deg);
}

.card-front,
.card-back {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 2rem;
  border: 2px solid #333;
  border-radius: 10px;
}

.card-front {
  background-color: #ccc;
}

.card-back {
  background-color: #fff;
  transform: rotateY(180deg);
}
</style>
