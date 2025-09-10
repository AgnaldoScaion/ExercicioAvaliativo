<template>
  <div class="home-page">
    <h1>🏠 Página Inicial</h1>
    <p>Bem-vindo ao exemplo de Vue Router!</p>
    
    <div class="welcome-card">
      <h3>O que você pode fazer aqui:</h3>
      <ul>
        <li>📦 Ver nossa lista de produtos</li>
        <li>ℹ️ Conhecer mais sobre nós</li>
        <li>📞 Entrar em contato</li>
      </ul>
    </div>

    <div class="quick-actions">
      <button @click="goToProducts" class="action-btn">
        Ver Produtos
      </button>
      <button @click="goToAbout" class="action-btn">
        Sobre Nós
      </button>
    </div>
  </div>
</template>

<script setup>
import { useRouter } from 'vue-router'

const router = useRouter()

const goToProducts = () => {
  router.push('/products')
}

const goToAbout = () => {
  router.push('/about')
}
</script>

<style scoped>
.home-page {
  text-align: center;
}

.welcome-card {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  margin: 20px 0;
  text-align: left;
}

.welcome-card h3 {
  color: #495057;
  margin-bottom: 15px;
}

.welcome-card ul {
  list-style: none;
  padding: 0;
}

.welcome-card li {
  padding: 8px 0;
  font-size: 16px;
}

.quick-actions {
  margin-top: 30px;
}

.action-btn {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 12px 24px;
  margin: 0 10px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s ease;
}

.action-btn:hover {
  background-color: #0056b3;
}
</style>
