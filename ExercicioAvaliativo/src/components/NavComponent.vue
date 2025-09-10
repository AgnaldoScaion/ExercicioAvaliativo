<template>
  <div class="router-example">
    <h2 style="display: flex; justify-content: center;">🚀 Exemplo Vue Router</h2>

    <nav class="navigation">
      <RouterLink to="/home" class="nav-link">🏠 Home</RouterLink>
      <RouterLink to="/products" class="nav-link">📦 Produtos</RouterLink>
      <RouterLink to="/about" class="nav-link">ℹ️ Sobre</RouterLink>
      <RouterLink to="/contact" class="nav-link">📞 Contato</RouterLink>
    </nav>


    <div class="content">
      <RouterView />
    </div>

  </div>
</template>

<script setup>
import { useRoute } from 'vue-router'

const route = useRoute()
</script>

<style scoped>
.router-example {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

.navigation {
  display: flex;
  gap: 15px;
  margin-bottom: 30px;
  padding: 15px;
  background-color: #f8f9fa;
  border-radius: 8px;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;

}

.nav-link {
  padding: 10px 15px;
  text-decoration: none;
  color: #495057;
  border-radius: 5px;
  transition: all 0.3s ease;
  font-weight: 500;
}

.nav-link:hover {
  background-color: #e9ecef;
  color: #007bff;
}

.nav-link.router-link-active {
  background-color: #007bff;
  color: white;
}

.content {
  min-height: 300px;
  padding: 20px;
  border: 2px solid #dee2e6;
  border-radius: 8px;
  margin-bottom: 20px;
  background-color: white;
}

.route-info {
  padding: 15px;
  background-color: #f8f9fa;
  border-radius: 8px;
  border-left: 4px solid #007bff;
}

.route-info h4 {
  margin-top: 0;
  color: #495057;
}

.route-info p {
  margin: 5px 0;
  font-family: monospace;
  background-color: white;
  padding: 5px;
  border-radius: 3px;
}
</style>
