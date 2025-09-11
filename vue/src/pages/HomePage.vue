<template>
  <div class="wrapper">
    <!-- Header Superior -->
    <div class="top-header">
      <div class="menu-icon" @click="toggleMenu">☰</div>
      <div class="user-header">
        <span>👤</span> {{ userName }}
      </div>
    </div>
    <!-- Logo -->
    <div class="header">
      <div class="header-img">
        <RouterLink to="/home">
          <img src="https://i.ibb.co/Q7T008b1/image.png" alt="Bella Avventura" class="floating" />
        </RouterLink>
      </div>
    </div>
    <!-- Conteúdo Principal -->
    <main class="container main-content">
      <!-- Container para o menu -->
      <div class="menu-container">
        <div class="menu-box" :class="{ hidden: !menuVisible, visible: menuVisible }">
          <div class="menu-lateral"></div>
          <div class="menu-conteudo">
            <h2>Menu</h2>
            <ul>
              <li><RouterLink to="/profile">Meu Perfil</RouterLink></li>
              <li><RouterLink to="/feedbacks">Meus Feedbacks</RouterLink></li>
              <li><RouterLink to="/restaurantes">Restaurantes</RouterLink></li>
              <li><RouterLink to="/hoteis">Hotéis</RouterLink></li>
              <li><RouterLink to="/pontos-turisticos">Pontos Turísticos</RouterLink></li>
              <li><RouterLink to="/termos">Termos e condições</RouterLink></li>
              <li><RouterLink to="/sobre">Sobre nós</RouterLink></li>
              <li v-if="isAuthenticated">
                <a href="#" @click.prevent="logout" class="logout-btn">Sair</a>
              </li>
            </ul>
          </div>
        </div>
      </div>
      <!-- Hero Section -->
      <section class="hero" id="hero">
        <div class="hero-content">
          <h1>Planeje sua viagem perfeita</h1>
          <p>Descubra os melhores restaurantes e pontos turísticos baseados na sua localização, avalie suas experiências e ganhe descontos para suas próximas aventuras!</p>
          <RouterLink :to="isAuthenticated ? '/destinos' : '/login'" class="btn">
            <i class="fas fa-compass"></i>
            {{ isAuthenticated ? 'Explorar Destinos' : 'Comece agora' }}
          </RouterLink>
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
            <p style="text-align: center; margin-bottom: 2rem;">
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
    </main>
    <!-- Rodapé -->
    <footer class="footer">
      <div class="footer-top">
        <RouterLink to="/home">
          <img src="https://i.ibb.co/j9vGknyy/image.png" alt="Bella Avventura" />
        </RouterLink>
      </div>
      <div class="footer-bottom">
        <div class="footer-left">
          <a href="mailto:bella.avventura@gmail.com">📧 bella.avventura@gmail.com</a>
        </div>
        <div class="footer-center">© 2025 Bella Avventura</div>
        <div class="footer-right">
          <RouterLink to="/termos">Termos e condições</RouterLink>
        </div>
      </div>
    </footer>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import { useRouter } from 'vue-router';
import { useAuth } from '@/composables/useAuth';

const router = useRouter();
const { isAuthenticated } = useAuth();

// Estado
const showBackToTop = ref(false);
const menuVisible = ref(false);
const userName = ref('Visitante');

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
]);

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
]);

// Métodos
const scrollToTop = () => {
  window.scrollTo({
    top: 0,
    behavior: 'smooth'
  });
};

const toggleMenu = () => {
  menuVisible.value = !menuVisible.value;
};

const logout = () => {
  localStorage.removeItem('currentUser');
  router.push('/home');
};

onMounted(() => {
  window.addEventListener('scroll', handleScroll);
  window.addEventListener('click', handleClickOutside);
  window.addEventListener('keydown', handleEscapeKey);
  handleScroll();
  const urlParams = new URLSearchParams(window.location.search);
  const msg = urlParams.get('msg');
  if (msg === 'login') {
    showNotification('Login realizado com sucesso!', 'success');
  } else if (msg === 'cadastro') {
    showNotification('Cadastro realizado com sucesso!', 'success');
  }
  const userData = JSON.parse(localStorage.getItem('currentUser'));
  if (userData) {
    userName.value = userData.firstName;
  }
  // Animar itens do menu
  const menuLinks = document.querySelectorAll('.menu-conteudo a');
  menuLinks.forEach((link, index) => {
    link.style.animationDelay = `${(index + 1) * 0.05}s`;
  });
});

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll);
  window.removeEventListener('click', handleClickOutside);
  window.removeEventListener('keydown', handleEscapeKey);
});

function handleScroll() {
  showBackToTop.value = window.pageYOffset > 400;
}

function handleClickOutside(e) {
  const menu = document.querySelector('.menu-box');
  const menuIcon = document.querySelector('.menu-icon');
  if (menuVisible.value && !menu.contains(e.target) && !menuIcon.contains(e.target)) {
    menuVisible.value = false;
  }
}

function handleEscapeKey(e) {
  if (e.key === 'Escape' && menuVisible.value) {
    menuVisible.value = false;
  }
}

function showNotification(message, type) {
  const notif = document.createElement('div');
  notif.className = `notification ${type}`;
  notif.textContent = message;
  document.body.appendChild(notif);
  setTimeout(() => {
    notif.classList.add('show');
  }, 10);
  setTimeout(() => {
    notif.classList.remove('show');
    setTimeout(() => notif.remove(), 300);
  }, 3000);
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&family=Inter:wght@300;400;500;600;700&display=swap');
:root {
  --primary-color: #2d5016;
  --primary-light: #5a8f3d;
  --primary-bg: #f3f7f3;
  --accent-color: #A7D096;
  --border-color: #E5F2E5;
  --text-dark: #1a1a1a;
  --text-medium: #4a4a4a;
  --text-light: #ffffff;
  --shadow-soft: 0 2px 15px rgba(45, 80, 22, 0.08);
  --shadow-medium: 0 8px 30px rgba(45, 80, 22, 0.12);
  --shadow-strong: 0 15px 40px rgba(45, 80, 22, 0.18);
  --transition-smooth: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  --border-radius: 16px;
  --border-radius-small: 8px;
}

/* Reset e Estilos Gerais */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.wrapper {
  flex: 1;
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  background-color: var(--primary-bg);
  color: var(--text-dark);
  font-family: 'Inter', sans-serif;
}

/* Header Superior */
.top-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 30px;
  background-color: #A7D096;
  position: relative;
  box-shadow: var(--shadow-soft);
}

.menu-icon {
  font-size: 24px;
  cursor: pointer;
  color: var(--primary-color);
  transition: var(--transition-smooth);
}

.menu-icon:hover {
  transform: scale(1.1);
}

.user-header {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 15px;
  font-weight: 600;
  color: var(--primary-color);
  background: rgba(255, 255, 255, 0.2);
  padding: 6px 12px;
  border-radius: 20px;
  transition: var(--transition-smooth);
}

.user-header:hover {
  background: rgba(255, 255, 255, 0.3);
  transform: translateY(-2px);
}

/* Header com Logo */
.header {
  background-color: #A7D096;
  position: relative;
  height: 86px;
}

.header-img img {
  height: 126px;
  transition: transform 0.5s ease;
}

.header-img {
  position: absolute;
  top: -50px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 1;
}

/* Menu */
.menu-container {
  position: relative;
  width: 100%;
  height: 0;
  overflow: visible;
}

.menu-box {
  position: absolute;
  top: 0;
  left: 0;
  background: linear-gradient(145deg, #ffffff, #f8fffe);
  border-radius: 20px;
  padding: 0;
  width: 320px;
  max-width: calc(100vw - 40px);
  display: flex;
  gap: 0;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.15), 0 10px 20px rgba(0, 0, 0, 0.1), 0 0 0 1px rgba(255, 255, 255, 0.8);
  font-family: 'Garamond', serif;
  z-index: 1000;
  transform: translateX(-100%) scale(0.9);
  opacity: 0;
  visibility: hidden;
  transition: all 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
  overflow: hidden;
  border: 1px solid rgba(76, 175, 80, 0.2);
}

.menu-box.visible {
  transform: translateX(0) scale(1);
  opacity: 1;
  visibility: visible;
}

.menu-lateral {
  background: linear-gradient(145deg, #4CAF50, #2E7D32);
  width: 8px;
  border-radius: 0;
  position: relative;
  box-shadow: inset 0 0 10px rgba(0, 0, 0, 0.1);
}

.menu-lateral::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 100%;
  background: linear-gradient(45deg, rgba(255, 255, 255, 0.3) 0%, transparent 50%, rgba(255, 255, 255, 0.1) 100%);
}

.menu-conteudo {
  flex: 1;
  padding: 25px 30px;
  background: transparent;
}

.menu-conteudo h2 {
  font-size: 28px;
  margin: 0 0 20px 0;
  border-bottom: 2px solid #E8F5E9;
  padding-bottom: 15px;
  color: #2E7D32;
  font-weight: 600;
  letter-spacing: 0.5px;
  position: relative;
}

.menu-conteudo h2::after {
  content: '✈️';
  position: absolute;
  right: 0;
  top: 0;
  font-size: 24px;
  animation: float 3s ease-in-out infinite;
}

.menu-conteudo ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.menu-conteudo li {
  margin: 0;
  transform: translateX(-20px);
  opacity: 0;
  animation: slideInItem 0.6s ease forwards;
}

.menu-conteudo li:nth-child(1) { animation-delay: 0.1s; }
.menu-conteudo li:nth-child(2) { animation-delay: 0.15s; }
.menu-conteudo li:nth-child(3) { animation-delay: 0.2s; }
.menu-conteudo li:nth-child(4) { animation-delay: 0.25s; }
.menu-conteudo li:nth-child(5) { animation-delay: 0.3s; }
.menu-conteudo li:nth-child(6) { animation-delay: 0.35s; }
.menu-conteudo li:nth-child(7) { animation-delay: 0.4s; }
.menu-conteudo li:nth-child(8) { animation-delay: 0.45s; }

@keyframes slideInItem {
  to {
    transform: translateX(0);
    opacity: 1;
  }
}

.menu-conteudo a {
  text-decoration: none;
  color: #424242;
  transition: all 0.3s ease;
  display: block;
  padding: 12px 15px;
  border-radius: 12px;
  font-size: 16px;
  font-weight: 500;
  position: relative;
  overflow: hidden;
  margin-bottom: 5px;
}

.menu-conteudo a::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(76, 175, 80, 0.1), transparent);
  transition: left 0.5s ease;
}

.menu-conteudo a:hover::before {
  left: 100%;
}

.menu-conteudo a:hover {
  color: #2E7D32;
  background: linear-gradient(135deg, #E8F5E9, #F1F8E9);
  transform: translateX(5px);
  box-shadow: 0 4px 12px rgba(76, 175, 80, 0.2);
}

.menu-conteudo a.logout-btn {
  color: #D32F2F !important;
  font-weight: 600;
  font-family: 'Inter', sans-serif;
}

.menu-conteudo a.logout-btn {
  background: linear-gradient(135deg, #FFEBEE, #FFCDD2);
  border: 1px solid rgba(211, 47, 47, 0.2);
  margin-top: 15px;
}

.menu-conteudo a.logout-btn:hover {
  background: linear-gradient(135deg, #FFCDD2, #EF9A9A);
  transform: translateX(5px);
  box-shadow: 0 4px 12px rgba(211, 47, 47, 0.3);
}

.hidden {
  visibility: hidden;
  opacity: 0;
  pointer-events: none;
}

.visible {
  visibility: visible;
  opacity: 1;
  pointer-events: auto;
}

/* Conteúdo Principal */
.container {
  max-width: 1200px;
  margin: 4rem auto 2rem;
  padding: 0 2rem;
}

.main-content {
  flex: 1 0 auto;
}

/* Hero Section */
.hero {
  background: linear-gradient(135deg, rgba(45, 80, 22, 0.8) 0%, rgba(90, 143, 61, 0.6) 100%),
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
  border-radius: var(--border-radius);
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

/* Button */
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
  position: relative;
  overflow: hidden;
}

.btn::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
  transition: left 0.5s;
}

.btn:hover::before {
  left: 100%;
}

.btn:hover {
  transform: translateY(-3px) scale(1.02);
  box-shadow: var(--shadow-strong);
  background: linear-gradient(135deg, var(--primary-light) 0%, var(--primary-color) 100%);
  color: var(--text-light);
}

/* Features Section */
.section {
  margin-bottom: 2rem;
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

.feature-card h3 {
  font-size: 1.4rem;
  font-weight: 600;
  color: var(--primary-color);
  margin-bottom: 1rem;
}

.feature-card p {
  color: var(--text-medium);
  font-weight: 400;
  line-height: 1.6;
}

/* Bonus System */
.bonus-system {
  background: var(--text-light);
  margin: 3rem 0;
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
  box-shadow: var(--shadow-strong);
  background: linear-gradient(135deg, var(--primary-light) 0%, var(--accent-color) 100%);
}

.back-to-top i {
  animation: bounce 2s infinite;
}

/* Footer */
.footer {
  background-color: #A7D096;
  padding: 20px;
  color: #000;
  font-size: 14px;
  text-align: center;
  width: 100%;
  flex-shrink: 0;
  box-shadow: var(--shadow-soft);
}

.footer-top {
  margin-bottom: 15px;
}

.footer-top img {
  width: 15%;
  height: auto;
  transition: transform 0.5s ease;
}

.footer-top:hover img {
  transform: rotate(5deg) scale(1.05);
}

.footer-bottom {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 10px;
}

.footer-left,
.footer-center,
.footer-right {
  flex: 1;
  text-align: center;
  transition: all 0.3s ease;
}

.footer-left:hover,
.footer-right:hover {
  transform: translateY(-3px);
}

.footer-left {
  text-align: left;
}

.footer-right {
  text-align: right;
}

.footer-bottom a {
  text-decoration: underline;
  color: #000;
}

.footer-bottom a:hover {
  color: var(--primary-color);
}

/* Notificação */
.notification {
  position: fixed;
  top: 20px;
  left: 50%;
  transform: translateX(-50%);
  padding: 15px 25px;
  border-radius: 5px;
  color: var(--text-light);
  font-weight: bold;
  z-index: 1000;
  opacity: 0;
  transition: opacity 0.3s ease, top 0.3s ease;
  max-width: 90%;
  text-align: center;
}

.notification.show {
  top: 30px;
  opacity: 1;
}

.notification.success {
  background-color: #4CAF50;
}

.notification.error {
  background-color: #F44336;
}

/* Animações */
@keyframes float {
  0%, 100% {
    transform: translateY(0px);
  }
  50% {
    transform: translateY(-5px);
  }
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

@keyframes bounce {
  0%, 20%, 50%, 80%, 100% {
    transform: translateY(0);
  }
  40% {
    transform: translateY(-5px);
  }
  60% {
    transform: translateY(-3px);
  }
}

/* Responsividade */
@media (max-width: 768px) {
  .container {
    padding: 0 1rem;
  }
  .hero {
    min-height: 80vh;
    background-attachment: scroll;
  }
  .section {
    margin-bottom: 1rem;
  }
  .features-grid {
    grid-template-columns: 1fr;
    gap: 1.5rem;
  }
  .feature-card {
    padding: 2rem;
  }
  .menu-box {
    width: calc(100vw - 30px);
    left: 15px;
    top: 60px;
  }
  .menu-conteudo {
    padding: 20px 25px;
  }
  .menu-conteudo h2 {
    font-size: 24px;
  }
  .menu-conteudo a {
    font-size: 15px;
    padding: 10px 12px;
  }
}

@media (max-width: 480px) {
  .container {
    padding: 0 1rem;
    margin: 2rem auto;
  }
  .hero {
    min-height: 70vh;
  }
  .hero h1 {
    font-size: 2rem;
  }
  .section-title {
    font-size: 1.8rem;
  }
  .feature-card {
    padding: 1.5rem;
  }
  .how-it-works {
    padding: 1rem;
  }
  .top-header {
    padding: 10px 15px;
  }
  .header-img img {
    height: 100px;
    top: -30px;
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
  .menu-box {
    width: calc(100vw - 20px);
    left: 10px;
    top: 50px;
  }
  .menu-conteudo {
    padding: 15px 20px;
  }
}
</style>
