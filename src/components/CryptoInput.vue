<script setup>
defineProps({
  amount: { type: [Number, String], required: true },
  onConvert: { type: Function, required: true },
  onFavorite: { type: Function, required: true },
})
const emit = defineEmits(['update:amount'])
</script>

<template>
  <div class="input-section">
    <input
      placeholder="Введите число:"
      min="0"
      type="number"
      :value="amount"
      @input="emit('update:amount', $event.target.value)"
    />
    <div class="buttons-flex">
      <button @click="onConvert" class="btn-convert">Конвертировать</button>
      <button @click="onFavorite" class="btn-fav">★ Избранное</button>
    </div>
  </div>
</template>

<style scoped>
.input-section {
  width: 100%;
  margin: 0 0 50px 0;
  padding: 0;
  text-align: center;
}

input {
  padding: 15px 20px;
  width: 100%;
  margin-bottom: 20px;
  font-size: 1.4em;
  border-radius: 8px;
  border: 0;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
  background-color: #fafafa;
  color: #333;
}

@media (max-width: 600px) {
  input {
    padding: 10px 15px;
    font-size: 1.1em;
  }

  .buttons-flex {
    gap: 10px;
  }
}

@media (max-width: 450px) {
  .buttons-flex {
    flex-direction: column;
    gap: 10px;
  }
  button {
    width: 100%;
  }
}

.buttons-flex {
  display: flex;
  justify-content: center;
  gap: 15px;
  flex-wrap: wrap;
}

button {
  flex-grow: 1;
  padding: 12px 20px;
  color: #fff;
  text-transform: uppercase;
  cursor: pointer;
  border: none;
  border-radius: 5px;
  transition:
    background-color 0.2s ease,
    transform 0.1s;
  font-weight: bold;
}

.btn-convert {
  background-color: #1a032d;
}

.btn-fav {
  background-color: #8c00ff;
}

button:hover {
  opacity: 0.9;
  transform: translateY(-1px);
}

@media (max-width: 450px) {
  .buttons-flex {
    flex-direction: column;
  }
  button {
    width: 100%;
  }
}
</style>
