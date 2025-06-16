<template>
  <div class="stats-overlay">
    <div class="stats-box">
      <h2>Game Over!</h2>
      <p>You completed the game in {{ moves }} moves.</p>
      <button @click="$emit('restart')">Play Again</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'GameStats',
  props: {
    moves: {
      type: Number,
      required: true
    }
  }
}
</script>

<style scoped>
.stats-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.6);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 10;
}

.stats-box {
  background: white;
  padding: 2rem;
  border-radius: 10px;
  text-align: center;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.3);
}
</style>
