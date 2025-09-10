<template>
  <div class="contact-page">
    <h1>📞 Entre em Contato</h1>
    <p>Estamos aqui para ajudar! Entre em contato conosco.</p>
    
    <div class="contact-container">
      <div class="contact-info">
        <h2>📍 Informações de Contato</h2>
        <div class="info-item">
          <span class="info-icon">📧</span>
          <div>
            <strong>Email:</strong>
            <p>contato@empresa.com</p>
          </div>
        </div>
        <div class="info-item">
          <span class="info-icon">📱</span>
          <div>
            <strong>Telefone:</strong>
            <p>(11) 99999-9999</p>
          </div>
        </div>
        <div class="info-item">
          <span class="info-icon">🏢</span>
          <div>
            <strong>Endereço:</strong>
            <p>Rua das Tecnologias, 123<br>São Paulo - SP</p>
          </div>
        </div>
        <div class="info-item">
          <span class="info-icon">🕒</span>
          <div>
            <strong>Horário:</strong>
            <p>Segunda a Sexta<br>9h às 18h</p>
          </div>
        </div>
      </div>

      <div class="contact-form">
        <h2>💬 Envie uma Mensagem</h2>
        <form @submit.prevent="submitForm">
          <div class="form-group">
            <label for="name">Nome:</label>
            <input 
              type="text" 
              id="name" 
              v-model="form.name" 
              required
              placeholder="Seu nome completo"
            >
          </div>
          
          <div class="form-group">
            <label for="email">Email:</label>
            <input 
              type="email" 
              id="email" 
              v-model="form.email" 
              required
              placeholder="seu@email.com"
            >
          </div>
          
          <div class="form-group">
            <label for="subject">Assunto:</label>
            <select id="subject" v-model="form.subject" required>
              <option value="">Selecione um assunto</option>
              <option value="suporte">Suporte Técnico</option>
              <option value="vendas">Vendas</option>
              <option value="parceria">Parceria</option>
              <option value="outros">Outros</option>
            </select>
          </div>
          
          <div class="form-group">
            <label for="message">Mensagem:</label>
            <textarea 
              id="message" 
              v-model="form.message" 
              required
              placeholder="Descreva como podemos ajudar..."
              rows="5"
            ></textarea>
          </div>
          
          <button type="submit" class="submit-btn" :disabled="isSubmitting">
            {{ isSubmitting ? 'Enviando...' : 'Enviar Mensagem' }}
          </button>
        </form>

        <div v-if="showSuccess" class="success-message">
          ✅ Mensagem enviada com sucesso! Entraremos em contato em breve.
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'

const form = reactive({
  name: '',
  email: '',
  subject: '',
  message: ''
})

const isSubmitting = ref(false)
const showSuccess = ref(false)

const submitForm = async () => {
  isSubmitting.value = true
  
  // Simular envio do formulário
  setTimeout(() => {
    isSubmitting.value = false
    showSuccess.value = true
    
    // Reset form
    Object.keys(form).forEach(key => {
      form[key] = ''
    })
    
    // Hide success message after 5 seconds
    setTimeout(() => {
      showSuccess.value = false
    }, 5000)
  }, 2000)
}
</script>

<style scoped>
.contact-page {
  max-width: 1000px;
  margin: 0 auto;
}

.contact-container {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 40px;
  margin-top: 30px;
}

@media (max-width: 768px) {
  .contact-container {
    grid-template-columns: 1fr;
    gap: 30px;
  }
}

.contact-info h2,
.contact-form h2 {
  color: #495057;
  margin-bottom: 20px;
}

.info-item {
  display: flex;
  align-items: flex-start;
  margin-bottom: 20px;
  padding: 15px;
  background-color: #f8f9fa;
  border-radius: 8px;
}

.info-icon {
  font-size: 1.5rem;
  margin-right: 15px;
  margin-top: 5px;
}

.info-item strong {
  color: #495057;
  display: block;
  margin-bottom: 5px;
}

.info-item p {
  margin: 0;
  color: #6c757d;
  line-height: 1.4;
}

.contact-form {
  background-color: white;
  padding: 25px;
  border: 1px solid #dee2e6;
  border-radius: 8px;
}

.form-group {
  margin-bottom: 20px;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
  color: #495057;
  font-weight: 500;
}

.form-group input,
.form-group select,
.form-group textarea {
  width: 100%;
  padding: 10px;
  border: 1px solid #ced4da;
  border-radius: 5px;
  font-size: 16px;
  transition: border-color 0.3s ease;
}

.form-group input:focus,
.form-group select:focus,
.form-group textarea:focus {
  outline: none;
  border-color: #007bff;
  box-shadow: 0 0 0 2px rgba(0, 123, 255, 0.25);
}

.form-group textarea {
  resize: vertical;
  font-family: inherit;
}

.submit-btn {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 12px 30px;
  border-radius: 5px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  width: 100%;
}

.submit-btn:hover:not(:disabled) {
  background-color: #0056b3;
}

.submit-btn:disabled {
  background-color: #6c757d;
  cursor: not-allowed;
}

.success-message {
  margin-top: 20px;
  padding: 15px;
  background-color: #d4edda;
  color: #155724;
  border: 1px solid #c3e6cb;
  border-radius: 5px;
  text-align: center;
}
</style>
