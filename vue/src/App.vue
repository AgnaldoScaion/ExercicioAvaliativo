<script setup>
import NavComponent from '../src/components/NavComponent.vue';
</script>

<template>
  <div class="app-container">
    <NavComponent></NavComponent>
    <main class="content-area">
      <slot>
        <!-- O conteúdo selecionado do NavComponent será exibido aqui -->
        <p>Selecione um item da navegação para visualizar o conteúdo.</p>
      </slot>
    </main>
  </div>
</template>

<style scoped>
/* Estilos do logo */
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}

/* Estilos para o layout no modo escuro */
.app-container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  width: 100%;
  background-color: #1a1a1a;
  color: #e0e0e0;
}

.content-area {
  flex: 1;
  padding: 2rem;
  max-width: 1200px;
  margin: 0 auto;
  width: 100%;
  box-sizing: border-box;
  background-color: #2c2c2c;
  border-radius: 8px;
}

/* Ajustes responsivos */
@media (max-width: 768px) {
  .content-area {
    padding: 1rem;
  }
}

@media (max-width: 480px) {
  .content-area {
    padding: 0.5rem;
  }
}
</style>