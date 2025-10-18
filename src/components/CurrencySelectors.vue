<script setup>
import CryptoSelector from './CryptoSelector.vue'

defineProps({
  cryptoFirst: { type: String, required: true },
  cryptoSecond: { type: String, required: true },
})

const emit = defineEmits(['update:cryptoFirst', 'update:cryptoSecond'])

const updateFirst = (val) => emit('update:cryptoFirst', val)
const updateSecond = (val) => emit('update:cryptoSecond', val)
</script>

<template>
  <div class="selectors">
    <CryptoSelector :modelValue="cryptoFirst" @update:modelValue="updateFirst" />
    <CryptoSelector :modelValue="cryptoSecond" @update:modelValue="updateSecond" />
  </div>
</template>

<style scoped>
.selectors {
  display: flex;
  justify-content: space-between;
  width: 100%;
  margin: 30px auto;
  padding: 0;
  gap: 20px;
}

@media (max-width: 600px) {
  .selectors {
    flex-direction: column;
    align-items: center;
    gap: 30px;
  }
}
</style>
