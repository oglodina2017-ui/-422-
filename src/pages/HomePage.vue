
<template>
  <div>Тут что-то будет!</div>
  <div class="home-page">
    <header class="page-header">
      <h1>Сетевой калькулятор</h1>
      <p>Расчет параметров подсетей по IP адресу и маске</p>
    </header>
    
    <main class="page-content">
      <NetworkCalculator />
    </main>
  </div>
</template>

<script setup lang="ts">
import NetworkCalculator from '../components/NetworkCalculator.vue'
</script>

<style scoped>
.home-page {
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  padding: 2rem;
}

.page-header {
  text-align: center;
  margin-bottom: 3rem;
  color: white;
}

.page-header h1 {
  margin: 0 0 1rem 0;
  font-size: 2.5rem;
}

.page-header p {
  margin: 0;
  font-size: 1.2rem;
  opacity: 0.9;
}

.page-content {
  display: flex;
  justify-content: center;
  align-items: flex-start;
}
</style>
