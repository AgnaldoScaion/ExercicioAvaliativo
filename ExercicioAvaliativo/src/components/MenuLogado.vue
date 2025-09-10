<template>
  <div>
    <!-- Overlay para escurecer o fundo quando o menu estiver aberto -->
    <div 
      class="menu-overlay" 
      :class="{ active: isMenuOpen }" 
      @click="closeMenu"
    ></div>

    <!-- Menu principal -->
    <div 
      id="menu-logado" 
      class="menu-box" 
      :class="{ visible: isMenuOpen }"
    >
      <div class="menu-lateral"></div>
      <div class="menu-conteudo">
        <h2>Menu</h2>
        <ul>
          <li><router-link to="/profile">Meu Perfil</router-link></li>
          <li><router-link to="/feedbacks">Meus Feedbacks</router-link></li>
          <li><router-link to="/restaurantes">Restaurantes</router-link></li>
          <li><router-link to="/hoteis">Hotéis</router-link></li>
          <li><router-link to="/pontos-turisticos">Pontos Turísticos</router-link></li>
          <li><router-link to="/termos">Termos e condições</router-link></li>
          <li><router-link to="/sobre-nos">Sobre nós</router-link></li>
          <li>
            <a href="#" @click.prevent="logout" class="logout-link">
              <span class="logout-btn">Sair</span>
            </a>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { useAuth } from '@/composables/useAuth'

const router = useRouter()
const { logout: authLogout } = useAuth()

const isMenuOpen = ref(false)

const closeMenu = () => {
  isMenuOpen.value = false
}

const logout = async () => {
  await authLogout()
  closeMenu()
  router.push('/login')
}

// Expor método para abrir menu
defineExpose({
  openMenu: () => isMenuOpen.value = true,
  closeMenu
})
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&family=Inter:wght@300;400;500;600;700&display=swap');

/* Overlay */
.menu-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.6);
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
  z-index: 999;
  backdrop-filter: blur(3px);
}

.menu-overlay.active {
  opacity: 1;
  visibility: visible;
}

/* Menu principal */
.menu-box {
  position: fixed;
  top: 70px;
  left: 20px;
  background: linear-gradient(145deg, #ffffff, #f8fffe);
  border-radius: 20px;
  padding: 0;
  width: 320px;
  max-width: calc(100vw - 40px);
  display: flex;
  gap: 0;
  box-shadow:
    0 20px 40px rgba(0, 0, 0, 0.15),
    0 10px 20px rgba(0, 0, 0, 0.1),
    0 0 0 1px rgba(255, 255, 255, 0.8);
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

/* Barra lateral decorativa */
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
  background: linear-gradient(45deg,
      rgba(255, 255, 255, 0.3) 0%,
      transparent 50%,
      rgba(255, 255, 255, 0.1) 100%);
}

/* Conteúdo do menu */
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

/* Lista de navegação */
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
  position: relative;
}

.menu-conteudo li:nth-child(1) { animation-delay: 0.1s; }
.menu-conteudo li:nth-child(2) { animation-delay: 0.15s; }
.menu-conteudo li:nth-child(3) { animation-delay: 0.2s; }
.menu-conteudo li:nth-child(4) { animation-delay: 0.25s; }
.menu-conteudo li:nth-child(5) { animation-delay: 0.3s; }
.menu-conteudo li:nth-child(6) { animation-delay: 0.35s; }
.menu-conteudo li:nth-child(7) { animation-delay: 0.4s; }
.menu-conteudo li:nth-child(8) { animation-delay: 0.45s; }

/* Links do menu */
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
  background: linear-gradient(90deg,
      transparent,
      rgba(76, 175, 80, 0.1),
      transparent);
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

/* Botão de logout */
.logout-btn {
  color: #D32F2F !important;
  font-weight: 600;
  position: relative;
  font-family: 'Inter', sans-serif;
}

.logout-link {
  background: linear-gradient(135deg, #FFEBEE, #FFCDD2);
  border: 1px solid rgba(211, 47, 47, 0.2);
  margin-top: 15px;
}

.logout-link:hover {
  background: linear-gradient(135deg, #FFCDD2, #EF9A9A);
  transform: translateX(5px);
  box-shadow: 0 4px 12px rgba(211, 47, 47, 0.3);
}

/* Animações */
@keyframes float {
  0%, 100% { transform: translateY(0px); }
  50% { transform: translateY(-5px); }
}

@keyframes slideInItem {
  to {
    transform: translateX(0);
    opacity: 1;
  }
}

/* Responsividade */
@media (max-width: 768px) {
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
