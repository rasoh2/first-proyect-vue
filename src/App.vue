<script setup>
import { ref, computed } from 'vue';

const counter = ref(0)
const arrayFavorite = ref([])

const increment = () => counter.value++
const decrement = () => counter.value--
const restart = () => {
  counter.value = 0;
  arrayFavorite.value = []
}
const add = () => arrayFavorite.value.push(counter.value)


const classCounter = () => {
  if (counter.value === 0) {
    return 'zero';
  }
  if (counter.value < 0) {
    return 'negative'
  }
  if (counter.value > 0) {
    return 'positive'
  }
};
const bloquearBtnAdd = computed(() => {
  const numSearch = arrayFavorite.value.find(num => num === counter.value)
  if (numSearch === 0) return true
  return numSearch ? true : false;
})
</script>


<template>
<div class="containerApp">
  <div class="counter-card">
    <div class="header">
      <h1 class="title">🎯 Contador Interactivo</h1>
      <p class="subtitle">Gestiona tus números favoritos</p>
    </div>

    <div class="counter-display">
      <div class="counter-value" :class="classCounter()">
        <span class="number-animation">{{ counter }}</span>
      </div>
      <div class="counter-label">Valor Actual</div>
    </div>

    <div class="controls">
      <button @click="increment" class="btn btn-success">
        <span class="btn-icon">➕</span>
        <span>Aumentar</span>
      </button>
      <button @click="decrement" class="btn btn-danger">
        <span class="btn-icon">➖</span>
        <span>Disminuir</span>
      </button>
      <button @click="add" class="btn btn-primary" :disabled='bloquearBtnAdd'>
        <span class="btn-icon">⭐</span>
        <span>Agregar</span>
      </button>
      <button @click='restart' class="btn btn-secondary">
        <span class="btn-icon">🔄</span>
        <span>Reiniciar</span>
      </button>
    </div>

    <div class="favorites-section" v-if="arrayFavorite.length > 0">
      <h3 class="favorites-title">📌 Números Favoritos</h3>
      <ul class="favorites-list">
        <li class="favorite-item" v-for="(num, index) in arrayFavorite" :key="index">
          <span class="favorite-number">{{ num }}</span>
        </li>
      </ul>
    </div>

    <div class="empty-state" v-else>
      <p>✨ Agrega números a tu lista de favoritos</p>
    </div>
  </div>
</div>
</template>


<style scoped>
.containerApp {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(135deg, #ff6b6b 0%, #ee5a6f 35%, #f4a261 70%, #e76f51 100%);
  padding: 20px;
}

.counter-card {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  border-radius: 24px;
  padding: 40px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
  max-width: 500px;
  width: 100%;
  animation: slideIn 0.5s ease-out;
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateY(30px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.header {
  text-align: center;
  margin-bottom: 30px;
}

.title {
  font-size: 2.5rem;
  font-weight: 700;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  margin-bottom: 10px;
  letter-spacing: -1px;
}

.subtitle {
  color: #64748b;
  font-size: 1rem;
  font-weight: 400;
}

.counter-display {
  text-align: center;
  margin: 30px 0;
  padding: 30px;
  background: linear-gradient(135deg, #f8fafc 0%, #e2e8f0 100%);
  border-radius: 16px;
  transition: transform 0.2s ease;
}

.counter-display:hover {
  transform: scale(1.02);
}

.counter-value {
  font-size: 5rem;
  font-weight: 800;
  line-height: 1;
  margin-bottom: 10px;
  transition: all 0.3s cubic-bezier(0.68, -0.55, 0.265, 1.55);
}

.number-animation {
  display: inline-block;
  animation: pulse 0.3s ease;
}

@keyframes pulse {

  0%,
  100% {
    transform: scale(1);
  }

  50% {
    transform: scale(1.1);
  }
}

.counter-label {
  font-size: 0.875rem;
  text-transform: uppercase;
  letter-spacing: 2px;
  color: #64748b;
  font-weight: 600;
}

.positive {
  color: #10b981;
  text-shadow: 0 0 20px rgba(16, 185, 129, 0.3);
}

.negative {
  color: #ef4444;
  text-shadow: 0 0 20px rgba(239, 68, 68, 0.3);
}

.zero {
  color: #f59e0b;
  text-shadow: 0 0 20px rgba(245, 158, 11, 0.3);
}

.controls {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 12px;
  margin-bottom: 30px;
}

.btn {
  padding: 16px 24px;
  font-size: 1rem;
  font-weight: 600;
  border: none;
  border-radius: 12px;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  position: relative;
  overflow: hidden;
}

.btn::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 0;
  height: 0;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.3);
  transform: translate(-50%, -50%);
  transition: width 0.6s, height 0.6s;
}

.btn:active::before {
  width: 300px;
  height: 300px;
}

.btn-icon {
  font-size: 1.2rem;
  transition: transform 0.3s ease;
}

.btn:hover .btn-icon {
  transform: scale(1.2) rotate(15deg);
}

.btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 15px rgba(0, 0, 0, 0.2);
}

.btn:active {
  transform: translateY(0);
}

.btn-success {
  background: linear-gradient(135deg, #10b981 0%, #059669 100%);
  color: white;
}

.btn-success:hover {
  background: linear-gradient(135deg, #059669 0%, #047857 100%);
}

.btn-danger {
  background: linear-gradient(135deg, #ef4444 0%, #dc2626 100%);
  color: white;
}

.btn-danger:hover {
  background: linear-gradient(135deg, #dc2626 0%, #b91c1c 100%);
}

.btn-primary {
  background: linear-gradient(135deg, #3b82f6 0%, #2563eb 100%);
  color: white;
}

.btn-primary:hover:not(:disabled) {
  background: linear-gradient(135deg, #2563eb 0%, #1d4ed8 100%);
}

.btn-primary:disabled {
  background: #cbd5e1;
  cursor: not-allowed;
  transform: none;
  box-shadow: none;
}

.btn-secondary {
  background: linear-gradient(135deg, #64748b 0%, #475569 100%);
  color: white;
}

.btn-secondary:hover {
  background: linear-gradient(135deg, #475569 0%, #334155 100%);
}

.favorites-section {
  animation: fadeIn 0.5s ease;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.favorites-title {
  font-size: 1.25rem;
  font-weight: 600;
  color: #334155;
  margin-bottom: 15px;
  text-align: center;
}

.favorites-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(70px, 1fr));
  gap: 10px;
  list-style: none;
  padding: 0;
  margin: 0;
}

.favorite-item {
  background: linear-gradient(135deg, #fbbf24 0%, #f59e0b 100%);
  padding: 15px;
  border-radius: 12px;
  text-align: center;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  animation: popIn 0.3s cubic-bezier(0.68, -0.55, 0.265, 1.55);
  transition: transform 0.2s ease;
}

.favorite-item:hover {
  transform: translateY(-3px) scale(1.05);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
}

@keyframes popIn {
  0% {
    opacity: 0;
    transform: scale(0.5);
  }

  100% {
    opacity: 1;
    transform: scale(1);
  }
}

.favorite-number {
  font-size: 1.5rem;
  font-weight: 700;
  color: white;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

.empty-state {
  text-align: center;
  padding: 30px;
  color: #94a3b8;
  font-style: italic;
  animation: fadeIn 0.5s ease;
}

@media (max-width: 640px) {
  .counter-card {
    padding: 24px;
  }

  .title {
    font-size: 2rem;
  }

  .counter-value {
    font-size: 4rem;
  }

  .controls {
    grid-template-columns: 1fr;
  }

  .favorites-list {
    grid-template-columns: repeat(auto-fill, minmax(60px, 1fr));
  }
}
</style>