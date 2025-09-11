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
      <section class="about-header">
        <h1>Sobre Nós</h1>
      </section>
      <div class="about-content">
        <section class="about-section">
          <h2>Nossa Missão</h2>
          <p>Na Bella Avventura, transformamos sonhos em experiências memoráveis. Desde 2020, conectamos viajantes aos destinos mais incríveis do mundo, oferecendo roteiros personalizados e serviços de alta qualidade.</p>
        </section>
        <section class="about-section">
          <h2>Nossa Equipe</h2>
          <p>Contamos com especialistas em viagens de mais de 15 países, todos apaixonados por explorar novas culturas e compartilhar conhecimento local autêntico.</p>
        </section>
        <section class="values-section">
          <article class="value-card">
            <h3>Experiência</h3>
            <p>Mais de 50 mil viagens realizadas com excelência</p>
          </article>
          <article class="value-card">
            <h3>Segurança</h3>
            <p>Parceria com os melhores fornecedores do mercado</p>
          </article>
          <article class="value-card">
            <h3>Inovação</h3>
            <p>Tecnologia exclusiva de planejamento de viagens</p>
          </article>
        </section>
        <section class="about-section">
          <h2>Compromisso Sustentável</h2>
          <p>Implementamos programas de turismo responsável em todos os destinos, garantindo que 5% de cada reserva seja reinvestido em comunidades locais.</p>
        </section>
      </div>
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

const menuVisible = ref(false);
const userName = ref('Visitante');

const toggleMenu = () => {
  menuVisible.value = !menuVisible.value;
};

const logout = () => {
  localStorage.removeItem('currentUser');
  router.push('/home');
};

onMounted(() => {
  window.addEventListener('click', handleClickOutside);
  window.addEventListener('keydown', handleEscapeKey);
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
  window.removeEventListener('click', handleClickOutside);
  window.removeEventListener('keydown', handleEscapeKey);
});

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

.about-header {
  text-align: center;
  margin-bottom: 4rem;
}

.about-header h1 {
  font-family: 'GaramondBold', serif;
  color: var(--primary-color);
  font-size: clamp(2rem, 4vw, 2.5rem);
  margin-bottom: 0.5rem;
}

.about-content {
  background: var(--text-light);
  padding: 2rem;
  border-radius: var(--border-radius);
  border: 2px solid var(--border-color);
  box-shadow: var(--shadow-soft);
}

.about-section {
  margin-bottom: 2rem;
}

.about-section h2 {
  color: var(--primary-color);
  font-family: 'GaramondBold', serif;
  font-size: 1.8rem;
  border-bottom: 2px solid var(--border-color);
  padding-bottom: 0.5rem;
  margin-bottom: 1.5rem;
}

.about-section p {
  color: var(--text-medium);
  line-height: 1.8;
  font-size: 1.1rem;
}

.values-section {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
  margin: 3rem 0;
}

.value-card {
  background: var(--text-light);
  padding: 2rem;
  border-radius: var(--border-radius);
  text-align: center;
  border: 2px solid var(--border-color);
  box-shadow: var(--shadow-soft);
  transition: var(--transition-smooth);
}

.value-card:hover {
  border-color: var(--accent-color);
  box-shadow: var(--shadow-medium);
  transform: translateY(-8px);
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

/* Responsividade */
@media (max-width: 768px) {
  .container {
    padding: 0 1rem;
  }
  .top-header {
    padding: 10px 20px;
  }
  .header-img img {
    height: 100px;
    top: -30px;
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
  .about-header h1 {
    font-size: 2rem;
  }
  .about-section h2 {
    font-size: 1.5rem;
  }
  .top-header {
    padding: 10px 15px;
  }
  .header-img img {
    height: 100px;
    top: -30px;
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
