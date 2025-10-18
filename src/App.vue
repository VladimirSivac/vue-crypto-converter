<script setup>
import { ref, watch } from 'vue'
import CryptoInput from './components/CryptoInput.vue'
import CurrencyResult from './components/CurrencyResult.vue'
import CurrencySelectors from './components/CurrencySelectors.vue'
import FavoriteList from './components/FavoriteList.vue'
import CryptoConvert from 'crypto-convert'
import { useStorage } from '@vueuse/core'

// --- State ---
let amount = ref(0)
let cryptoFirst = ref('BTC')
let cryptoSecond = ref('ETH')
let error = ref('')
let result = ref(0)

let favs = useStorage('cryptoFavs', [])

const convert = new CryptoConvert()

// --- Handlers & Watchers ---
watch([amount, cryptoFirst, cryptoSecond], () => {
  if (result.value !== 0) result.value = 0
  error.value = ''
})

// --- Actions (Логика конвертации) ---
const convertCurrency = async () => {
  error.value = ''
  result.value = 0

  // 1. Валидация
  if (amount.value <= 0) return (error.value = 'Введите число больше 0')
  if (!cryptoFirst.value || !cryptoSecond.value) return (error.value = 'Выберите валюту')
  if (cryptoFirst.value === cryptoSecond.value) return (error.value = 'Выберите другую валюту')

  // 2. Конвертация
  try {
    await convert.ready()
    const from = cryptoFirst.value.toUpperCase()
    const to = cryptoSecond.value.toUpperCase()
    const conversionFunction = convert[from][to]

    if (conversionFunction) {
      result.value = conversionFunction(amount.value)
    } else {
      error.value = 'Не удалось найти курс для выбранной пары.'
    }
  } catch (err) {
    console.error('Ошибка конвертации:', err)
    error.value = 'Произошла ошибка при расчете курса.'
  }
}

// --- Favorites ---
const addToFavorite = () => {
  const newFav = { from: cryptoFirst.value, to: cryptoSecond.value }

  const isDuplicate = favs.value.some((f) => f.from === newFav.from && f.to === newFav.to)

  if (isDuplicate) return (error.value = 'Эта пара уже в избранном')

  favs.value.push(newFav)
}

const loadFromFavs = (favPair) => {
  cryptoFirst.value = favPair.from
  cryptoSecond.value = favPair.to
  convertCurrency()
}
</script>

<template>
  <div class="converter-app">
    <h1>CRYPTO CONVERTER</h1>

    <div class="main-content-wrapper">
      <CryptoInput
        @update:amount="amount = $event"
        :amount="amount"
        :onConvert="convertCurrency"
        :onFavorite="addToFavorite"
      />

      <CurrencyResult
        :error="error"
        :result="result"
        :amount="amount"
        :cryptoFirst="cryptoFirst"
        :cryptoSecond="cryptoSecond"
      />

      <FavoriteList :favs="favs" :onSelect="loadFromFavs" v-if="favs.length > 0" />

      <CurrencySelectors
        :cryptoFirst="cryptoFirst"
        :cryptoSecond="cryptoSecond"
        @update:cryptoFirst="cryptoFirst = $event"
        @update:cryptoSecond="cryptoSecond = $event"
      />
    </div>
  </div>
</template>

<style scoped>
.main-content-wrapper {
  max-width: 700px;
  width: 90%;
  margin: 0 auto;
  padding: 0;
}
</style>
