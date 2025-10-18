<script setup>
defineProps({
  modelValue: {
    type: String,
    required: true,
  },
})

const emit = defineEmits(['update:modelValue'])

const availableCryptos = ['BTC', 'ETH', 'USDT', 'USD']

const selectItem = (val) => {
  emit('update:modelValue', val)
}
</script>

<template>
  <ul class="crypto-list">
    <li
      v-for="crypto in availableCryptos"
      :key="crypto"
      @click="selectItem(crypto)"
      :class="{ active: modelValue === crypto }"
    >
      {{ crypto }}
    </li>
  </ul>
</template>

<style scoped>
ul {
  list-style: none;
  width: 100%;
  padding: 0;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
}

li {
  display: block;
  background-color: #1a032d;
  color: #fff;
  padding: 15px 0;
  text-align: center;
  border-bottom: 1px solid #24033e;
}

li:last-child {
  border-bottom: none;
}

li:hover,
li.active {
  background-color: #3b0666;
  cursor: pointer;
  font-weight: bold;
}
</style>
