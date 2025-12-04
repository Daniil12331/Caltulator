<template>
  <div class="calculator">
    <h2>Калькулятор подсетей</h2>

    <div class="input-group">
      <label for="ip">IP-адрес:</label>
      <input
        id="ip"
        v-model="ipInput"
        type="text"
        placeholder="192.168.1.150"
        @keyup.enter="calculate"
      />
    </div>

    <div class="input-group">
      <label for="mask">Маска подсети:</label>
      <select id="mask" v-model="selectedMask">
        <option v-for="mask in SUBNET_MASKS" :key="mask" :value="mask">
          {{ mask }}
        </option>
      </select>
    </div>

    <button
      :disabled="!isValid"
      @click="calculate"
      class="calculate-btn"
    >
      Рассчитать
    </button>

    <div v-if="result" class="result">
      <p><strong>IP:</strong> {{ result.ip }}</p>
      <p><strong>Маска:</strong> {{ result.mask }}</p>
      <p><strong>Адрес сети:</strong> {{ result.network }}</p>
      <p><strong>Количество хостов:</strong> {{ result.hosts }}</p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import { SUBNET_MASKS } from '../constants/subnetMasks';
import { isIpValid } from '../utils/ipValidation';
import { getNetworkAddress } from '../utils/networkAddress';
import { getAddressesCount } from '../utils/addressesCount';

const ipInput = ref('');
const selectedMask = ref(SUBNET_MASKS[24]); // по умолчанию /24 → 255.255.255.0
const result = ref<{ ip: string; mask: string; network: string; hosts: number } | null>(null);

const isValid = computed(() => isIpValid(ipInput.value));

function calculate() {
  if (!isValid.value) return;

  const network = getNetworkAddress(ipInput.value, selectedMask.value);
  const hosts = getAddressesCount(selectedMask.value);

  result.value = {
    ip: ipInput.value,
    mask: selectedMask.value,
    network,
    hosts
  };
}
</script>

<style scoped>
.calculator {
  max-width: 500px;
  margin: 2rem auto;
  padding: 1.5rem;
  background-color: var(--bg-color);
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}

.input-group {
  margin-bottom: 1rem;
}

.input-group label {
  display: block;
  margin-bottom: 0.4rem;
  font-weight: 600;
  color: var(--text-color);
}

.input-group input,
.input-group select {
  width: 100%;
  padding: 0.5rem;
  border: 1px solid var(--border-color);
  border-radius: 4px;
  font-size: 1rem;
}

.calculate-btn {
  width: 100%;
  padding: 0.75rem;
  background-color: var(--primary-color);
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.2s;
}

.calculate-btn:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

.result {
  margin-top: 1.5rem;
  padding: 1rem;
  background-color: var(--result-bg);
  border-radius: 4px;
  font-family: monospace;
}
</style>