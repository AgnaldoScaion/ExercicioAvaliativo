<template>
  <!-- Hero Section -->
  <section class="hero" id="hero">
    <div class="hero-content">
      <h1>Planeje sua viagem perfeita</h1>
      <p>Descubra os melhores restaurantes e pontos turísticos baseados na sua localização, avalie suas experiências e
        ganhe descontos para suas próximas aventuras!</p>
      <router-link :to="isAuthenticated ? '/destinos' : '/login'" class="btn">
        <i class="fas fa-compass"></i>
        {{ isAuthenticated ? 'Explorar Destinos' : 'Comece agora' }}
      </router-link>
    </div>
  </section>

  <!-- Features Section -->
  <section class="section" id="features">
    <h2 class="section-title">Como o Bella Avventura funciona</h2>
    <div class="features-grid">
      <div class="feature-card" v-for="feature in features" :key="feature.title">
        <i :class="feature.icon + ' feature-icon'"></i>
        <h3>{{ feature.title }}</h3>
        <p>{{ feature.description }}</p>
      </div>
    </div>
  </section>

  <!-- Bonus System Section -->
  <section class="section" id="bonus">
    <div class="bonus-system">
      <div class="how-it-works">
        <h2 class="section-title">Sistema de Bonificação</h2>

        <div class="formula-card">
          <i class="fas fa-calculator" style="margin-right: 0.5rem;"></i>
          Fórmula: (Avaliações × 5) + (Qualidade × 10) + (Engajamento × 3)
        </div>

        <p style="text-align: center; margin-bottom: 2rem; color: var(--text-medium);">
          Seus pontos são calculados com base em:
        </p>

        <ul class="points-list">
          <li v-for="point in bonusPoints" :key="point.title">
            <strong>{{ point.title }}:</strong> {{ point.description }}
          </li>
        </ul>

        <div class="example-card">
          <p><strong>Exemplo prático:</strong></p>
          <p>5 avaliações com qualidade 4 e engajamento 3:</p>
          <p class="calculation">
            (5 × 5) + (4 × 10) + (3 × 3) = 25 + 40 + 9 = <strong>74 pontos</strong>
          </p>
        </div>
      </div>
    </div>
  </section>

  <!-- Back to Top Button -->
  <button 
    class="back-to-top" 
    :class="{ visible: showBackToTop }" 
    @click="scrollToTop"
    id="backToTop">
    <i class="fas fa-arrow-up"></i>
  </button>
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
