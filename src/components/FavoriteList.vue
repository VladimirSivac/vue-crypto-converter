<script setup>
defineProps({
  favs: {
    type: Array,
    required: true,
  },

  onSelect: {
    type: Function,
    required: true,
  },
})
</script>

<template>
  <div class="favs-container">
    <h2>★ Избранные пары:</h2>
    <div class="fav-item" v-for="el in favs" :key="`${el.from}-${el.to}`" @click="onSelect(el)">
      <span class="text">{{ el.from }}</span>
      <span class="arrow">→</span>
      <span class="text">{{ el.to }}</span>
    </div>
  </div>
</template>

<style scoped>
.favs-container {
  margin: 20px auto;
  width: 90%;
  max-width: 700px;
  padding: 10px;
  background: rgba(255, 255, 255, 0.05);
  border-radius: 8px;
}

h2 {
  color: #fff;
  font-size: 1.2em;
  margin-bottom: 15px;
}

.fav-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  max-width: 400px;
  margin: 10px auto;
  transition:
    transform 300ms ease,
    opacity 300ms ease;
  cursor: pointer;
}

.fav-item:hover {
  opacity: 0.8;
  transform: translateY(-2px);
}

.text {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-grow: 1;
  flex-basis: 0;
  padding: 8px 12px;
  background-color: #8c00ff;
  color: #fff;
  border-radius: 4px;
  font-weight: bold;
}

.arrow {
  font-size: 1.5em;
  color: #fff;
  margin: 0 10px;
  flex-shrink: 0;
}
</style>
