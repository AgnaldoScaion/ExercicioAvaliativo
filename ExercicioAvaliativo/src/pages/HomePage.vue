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
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { useRouter } from 'vue-router'
import { useAuth } from '@/composables/useAuth'

const router = useRouter()
const { isAuthenticated } = useAuth()

// Estado
const showBackToTop = ref(false)

// Dados
const features = ref([
  {
    icon: 'fas fa-map-marker-alt',
    title: 'Descubra locais próximos',
    description: 'Nosso sistema mostra automaticamente os melhores restaurantes e pontos turísticos próximos ao seu hotel.'
  },
  {
    icon: 'fas fa-star',
    title: 'Avalie e ganhe pontos',
    description: 'Acumule pontos com suas avaliações que podem ser convertidos em descontos exclusivos.'
  },
  {
    icon: 'fas fa-share-alt',
    title: 'Compartilhe experiências',
    description: 'Ajude outros viajantes compartilhando suas avaliações e descobertas.'
  }
])

const bonusPoints = ref([
  {
    title: 'Avaliações',
    description: '5 pontos por cada avaliação completa'
  },
  {
    title: 'Qualidade',
    description: '10 pontos por nível (1-5) da sua avaliação'
  },
  {
    title: 'Engajamento',
    description: '3 pontos por nível (1-5) de detalhes fornecidos'
  }
])

// Métodos
const scrollToTop = () => {
  window.scrollTo({
    top: 0,
    behavior: 'smooth'
  })
}

const handleScroll = () => {
  showBackToTop.value = window.pageYOffset > 400
}

// Lifecycle hooks
onMounted(() => {
  window.addEventListener('scroll', handleScroll)
  handleScroll()
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>

<style scoped>
/* Hero Section */
.hero {
  background: linear-gradient(135deg,
      rgba(45, 80, 22, 0.8) 0%,
      rgba(90, 143, 61, 0.6) 100%),
    url('https://images.unsplash.com/photo-1506929562872-bb421503ef21?ixlib=rb-1.2.1&auto=format&fit=crop&w=1920&q=80');
  background-size: cover;
  background-position: center;
  background-attachment: fixed;
  min-height: 90vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  color: var(--text-light);
  position: relative;
}

.hero::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: radial-gradient(circle at center, transparent 0%, rgba(45, 80, 22, 0.3) 100%);
}

.hero-content {
  position: relative;
  z-index: 2;
  max-width: 900px;
  padding: 0 20px;
  animation: fadeInUp 1s ease-out;
}

.hero h1 {
  font-family: 'GaramondBold', serif;
  font-size: clamp(2.5rem, 5vw, 4rem);
  margin-bottom: 1.5rem;
  font-weight: 700;
  letter-spacing: -0.02em;
  text-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
}

.hero p {
  font-size: clamp(1.1rem, 2vw, 1.3rem);
  margin-bottom: 2.5rem;
  font-weight: 400;
  opacity: 0.95;
  line-height: 1.7;
  text-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
}

/* Features Section */
.section {
  padding: 6rem 2rem;
  max-width: 1200px;
  margin: 0 auto;
}

.section-title {
  font-family: 'GaramondBold', serif;
  font-size: clamp(2rem, 4vw, 2.5rem);
  text-align: center;
  margin-bottom: 3rem;
  color: var(--primary-color);
  position: relative;
}

.section-title::after {
  content: '';
  position: absolute;
  bottom: -1rem;
  left: 50%;
  transform: translateX(-50%);
  width: 60px;
  height: 3px;
  background: linear-gradient(90deg, var(--accent-color), var(--primary-light));
  border-radius: 2px;
}

.features-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
  margin-top: 4rem;
}

.feature-card {
  background: var(--text-light);
  border-radius: var(--border-radius);
  padding: 2.5rem;
  text-align: center;
  border: 2px solid var(--border-color);
  box-shadow: var(--shadow-soft);
  transition: var(--transition-smooth);
  position: relative;
  overflow: hidden;
}

.feature-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 4px;
  background: linear-gradient(90deg, var(--accent-color), var(--primary-light));
  transform: scaleX(0);
  transition: transform 0.3s ease;
}

.feature-card:hover::before {
  transform: scaleX(1);
}

.feature-card:hover {
  transform: translateY(-8px);
  box-shadow: var(--shadow-medium);
  border-color: var(--accent-color);
}

.feature-icon {
  font-size: 3rem;
  color: var(--primary-light);
  margin-bottom: 1.5rem;
  display: block;
}

/* Bonus System */
.bonus-system {
  background: var(--text-light);
  margin: 4rem 0;
  border-radius: var(--border-radius);
  box-shadow: var(--shadow-soft);
  overflow: hidden;
  padding: 2rem;
}

.formula-card {
  background: linear-gradient(135deg, var(--accent-color) 0%, var(--primary-light) 100%);
  color: var(--text-dark);
  padding: 2rem;
  text-align: center;
  font-size: 1.3rem;
  font-weight: 700;
  box-shadow: var(--shadow-soft);
  margin: 2rem 0;
  border-radius: var(--border-radius-small);
}

.points-list {
  list-style: none;
  padding: 0;
  margin: 2rem 0;
}

.points-list li {
  display: flex;
  align-items: center;
  padding: 1rem;
  margin-bottom: 1rem;
  background: var(--primary-bg);
  border-radius: var(--border-radius-small);
  border-left: 4px solid var(--accent-color);
}

.points-list li::before {
  content: '✓';
  color: var(--primary-light);
  font-weight: bold;
  margin-right: 1rem;
  font-size: 1.2rem;
}

.example-card {
  background: var(--primary-bg);
  padding: 1.5rem;
  border-radius: var(--border-radius-small);
  border: 1px solid var(--border-color);
  margin-top: 2rem;
}

.example-card strong {
  color: var(--primary-color);
}

.btn {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  background: linear-gradient(135deg, var(--accent-color) 0%, var(--primary-light) 100%);
  color: var(--text-dark);
  padding: 1rem 2rem;
  border: none;
  border-radius: var(--border-radius-small);
  font-size: 1.1rem;
  font-weight: 600;
  cursor: pointer;
  transition: var(--transition-smooth);
  text-decoration: none;
  box-shadow: var(--shadow-medium);
}

.btn:hover {
  transform: translateY(-3px) scale(1.02);
  box-shadow: var(--shadow-strong);
  background: linear-gradient(135deg, var(--primary-light) 0%, var(--primary-color) 100%);
  color: var(--text-light);
}

/* Back to Top Button */
.back-to-top {
  position: fixed;
  bottom: 2rem;
  right: 2rem;
  width: 60px;
  height: 60px;
  background: linear-gradient(135deg, var(--primary-color) 0%, var(--primary-light) 100%);
  color: var(--text-light);
  border: none;
  border-radius: 50%;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.5rem;
  box-shadow: var(--shadow-medium);
  transition: var(--transition-smooth);
  opacity: 0;
  visibility: hidden;
  transform: scale(0.8);
  z-index: 1000;
}

.back-to-top.visible {
  opacity: 1;
  visibility: visible;
  transform: scale(1);
}

.back-to-top:hover {
  transform: scale(1.1) translateY(-3px);
  background: linear-gradient(135deg, var(--primary-light) 0%, var(--accent-color) 100%);
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* Responsive Design */
@media (max-width: 768px) {
  .hero {
    min-height: 80vh;
    background-attachment: scroll;
  }

  .section {
    padding: 4rem 1rem;
  }

  .features-grid {
    grid-template-columns: 1fr;
    gap: 1.5rem;
  }
}

@media (max-width: 480px) {
  .hero {
    min-height: 70vh;
  }

  .back-to-top {
    width: 50px;
    height: 50px;
    bottom: 1rem;
    right: 1rem;
    font-size: 1.2rem;
  }

  .btn {
    padding: 0.8rem 1.5rem;
    font-size: 1rem;
  }
}
</style>
