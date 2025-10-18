<script setup>
defineProps({
  error: { type: String, required: true },
  result: { type: [Number, String], required: true },
  amount: { type: [Number, String], required: true },
  cryptoFirst: { type: String, required: true },
  cryptoSecond: { type: String, required: true },
})

const getFormattedResult = (val) => {
  if (val === 0) return '0.00'
  if (val < 0.01) return val.toFixed(8)
  return val.toFixed(2)
}
</script>

<template>
  <div class="result-section">
    <p v-if="error" class="error-message">{{ error }}</p>

    <p v-else-if="result !== 0" class="result-text">
      {{ amount }} {{ cryptoFirst }} = {{ getFormattedResult(result) }} {{ cryptoSecond }}
    </p>

    <p v-else class="initial-message">Введите сумму и выберите валюты для конвертации.</p>
  </div>
</template>

<style scoped>
.result-section {
  min-height: 80px;
  width: 100%;
  margin: 20px 0;
  padding: 0;
}

.error-message,
.result-text,
.initial-message {
  text-align: center;
  word-wrap: break-word;
  overflow-wrap: break-word;
  padding: 0 10px;
}

.error-message {
  color: #ff4d4f;
  font-weight: bold;
  font-size: 1.2em;
}

.result-text {
  font-family: 'Nabla', cursive;
  font-size: 2.5em;
  color: #fff;
  text-shadow: 0 0 10px #8c00ff;
}

.initial-message {
  color: rgba(255, 255, 255, 0.7);
  font-style: italic;
}

@media (max-width: 600px) {
  .result-text {
    font-size: 1.8em;
  }
}
</style>
