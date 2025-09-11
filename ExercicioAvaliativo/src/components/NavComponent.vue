<template>
  <div class="router-example">
    <h2 style="display: flex; justify-content: center;">🚀 Exemplo Vue Router </h2>
    <p style="display: flex; justify-content: center;">(Sim eu copiei do Daniboy :D)</p>
    <nav class="navigation">
      <RouterLink to="/home" class="nav-link">🏠 Home</RouterLink>
      <RouterLink to="/sobre" class="nav-link">ℹ️ Sobre</RouterLink>
    </nav>
    <div class="content">
      <RouterView />
    </div>
  </div>
</template>

<script setup>
import { useRoute } from 'vue-router';
const route = useRoute();
</script>

<style scoped>
.router-example {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
  font-family: Arial, sans-serif;
  background-color: #1a1a1a;
  color: #e0e0e0;
}

.navigation {
  display: flex;
  gap: 1rem;
  margin-bottom: 2rem;
  padding: 1rem;
  background-color: #2c2c2c;
  border-radius: 8px;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
}

.nav-link {
  padding: 0.8rem 1.5rem;
  text-decoration: none;
  color: #e0e0e0;
  border-radius: 5px;
  transition: all 0.3s ease;
  font-weight: 500;
}

.nav-link:hover {
  background-color: #444444;
  color: #42b883;
}

.nav-link.router-link-active {
  background-color: #42b883;
  color: #1a1a1a;
}

.content {
  min-height: 500px;
  padding: 2rem;
  border: 2px solid #444444;
  border-radius: 8px;
  margin-bottom: 2rem;
  background-color: #2c2c2c;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
}

.route-info {
  padding: 1rem;
  background-color: #333333;
  border-radius: 8px;
  border-left: 4px solid #646cff;
}

.route-info h4 {
  margin-top: 0;
  color: #e0e0e0;
}

.route-info p {
  margin: 5px 0;
  font-family: monospace;
  background-color: #2c2c2c;
  padding: 5px;
  border-radius: 3px;
  color: #e0e0e0;
}

/* Ajustes responsivos */
@media (max-width: 768px) {
  .router-example {
    padding: 1rem;
  }

  .content {
    padding: 1rem;
    min-height: 400px;
  }
}

@media (max-width: 480px) {
  .router-example {
    padding: 0.5rem;
  }

  .navigation {
    gap: 0.5rem;
    padding: 0.5rem;
  }

  .nav-link {
    padding: 0.6rem 1rem;
    font-size: 0.9rem;
  }

  .content {
    padding: 0.5rem;
    min-height: 300px;
  }
}
</style>