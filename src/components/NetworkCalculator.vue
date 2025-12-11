<template>
  <div class="calculator">
    <h2 class="calculator-title">Калькулятор подсетей</h2>
    
    <div class="input-group">
      <label class="input-label" for="ip-address">IP адрес:</label>
      <input
        id="ip-address"
        v-model="ipAddress"
        type="text"
        placeholder="192.168.1.150"
        class="input-field"
        :class="{ error: !isIpValid && ipAddress }"
        @keyup.enter="calculate"
      />
      <div v-if="!isIpValid && ipAddress" class="error-message">
        Введите корректный IP адрес
      </div>
    </div>

    <div class="input-group">
      <label class="input-label" for="network-mask">Сетевая маска:</label>
      <select
        id="network-mask"
        v-model="selectedMask"
        class="select-field"
      >
        <option
          v-for="option in MASK_OPTIONS"
          :key="option.cidr"
          :value="option.value"
        >
          {{ option.label }}
        </option>
      </select>
    </div>

    <button
      class="calculate-button"
      :disabled="!isFormValid"
      @click="calculate"
    >
      Рассчитать
    </button>

    <div v-if="showResults" class="results">
      <h3 class="results-title">Результаты расчета:</h3>
      
      <div class="result-item">
        <span class="result-label">IP адрес:</span>
        <span class="result-value">{{ ipAddress }}</span>
      </div>
      
      <div class="result-item">
        <span class="result-label">Маска сети:</span>
        <span class="result-value">{{ selectedMaskLabel }}</span>
      </div>
      
      <div class="result-item">
        <span class="result-label">Адрес сети:</span>
        <span class="result-value">{{ networkAddress }}</span>
      </div>
      
      <div class="result-item">
        <span class="result-label">Количество адресов:</span>
        <span class="result-value">{{ addressesCount }}</span>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import { MASK_OPTIONS } from '../constants/maskOptions'
import { isIpValid } from '../utils/ipValidation'
import { getNetworkAddress } from '../utils/networkCalculations'
import { getAddressesCount } from '../utils/addressCount'

const ipAddress = ref('192.168.1.150')
const selectedMask = ref('255.255.255.0')
const showResults = ref(false)
const networkAddress = ref('')
const addressesCount = ref(0)

const isFormValid = computed(() => {
  return isIpValid(ipAddress.value)
})

const selectedMaskLabel = computed(() => {
  const mask = MASK_OPTIONS.find(option => option.value === selectedMask.value)
  return mask ? mask.label : ''
})

const calculate = () => {
  if (!isFormValid.value) return
  
  networkAddress.value = getNetworkAddress(ipAddress.value, selectedMask.value)
  addressesCount.value = getAddressesCount(selectedMask.value)
  showResults.value = true
}
</script>

<style scoped>
.calculator {
  max-width: 500px;
  margin: 0 auto;
  padding: 2rem;
  background: var(--color-white);
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.calculator-title {
  text-align: center;
  color: var(--color-black);
  margin-bottom: 2rem;
  font-size: 1.5rem;
  font-weight: 700;
}

.input-group {
  margin-bottom: 1.5rem;
}

.input-label {
  display: block;
  margin-bottom: 0.5rem;
  color: var(--color-black);
  font-weight: 500;
}

.input-field {
  width: 100%;
  padding: 0.75rem;
  border: 2px solid var(--color-gray);
  border-radius: 4px;
  font-size: 1rem;
  transition: border-color 0.3s ease;
}

.input-field:focus {
  outline: none;
  border-color: var(--color-primary);
}

.input-field.error {
  border-color: var(--color-error);
}

.select-field {
  width: 100%;
  padding: 0.75rem;
  border: 2px solid var(--color-gray);
  border-radius: 4px;
  font-size: 1rem;
  background: var(--color-white);
  cursor: pointer;
}

.select-field:focus {
  outline: none;
  border-color: var(--color-primary);
}

.calculate-button {
  width: 100%;
  padding: 0.75rem;
  background: var(--color-primary);
  color: var(--color-white);
  border: none;
  border-radius: 4px;
  font-size: 1rem;
  font-weight: 500;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.calculate-button:hover:not(:disabled) {
  background: #005a9c;
}

.calculate-button:disabled {
  background: var(--color-gray);
  cursor: not-allowed;
}

.error-message {
  color: var(--color-error);
  font-size: 0.875rem;
  margin-top: 0.25rem;
}

.results {
  margin-top: 2rem;
  padding: 1.5rem;
  background: var(--color-gray);
  border-radius: 4px;
  border-left: 4px solid var(--color-primary);
}

.results-title {
  margin-top: 0;
  margin-bottom: 1rem;
  color: var(--color-black);
}

.result-item {
  margin-bottom: 0.5rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.result-label {
  font-weight: 500;
  color: var(--color-black);
}

.result-value {
  font-family: 'Courier New', monospace;
  color: var(--color-success);
}
</style>s
