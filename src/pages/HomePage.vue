<template>
  <div class="home">
    <SubnetCalculator />
  </div>
</template>

<script setup lang="ts">
import SubnetCalculator from '../components/SubnetCalculator.vue';
</script>

<style scoped>
.home {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  padding: 2rem 1rem;
}
</style>