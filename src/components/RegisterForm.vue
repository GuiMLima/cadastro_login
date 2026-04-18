<template>
  <div class="form-container">
    <div class="form-header">
      <h2>{{ isLoginMode ? 'Login Renda Fácil' : 'Cadastro Renda Fácil' }}</h2>
      <p>
        {{
          isLoginMode
            ? 'Acesse sua conta para continuar no simulador.'
            : 'Crie sua conta para acessar o simulador.'
        }}
      </p>
    </div>

    <form @submit.prevent="handleSubmit" class="register-form">
      <div class="input-group">
        <label for="email">Email</label>
        <div class="input-wrapper">
          <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect width="20" height="16" x="2" y="4" rx="2"/><path d="m22 7-8.97 5.7a1.94 1.94 0 0 1-2.06 0L2 7"/></svg>
          <input 
            type="email" 
            id="email" 
            v-model="formData.email" 
            @input="clearErrors"
            placeholder="seu@email.com" 
            required 
          />
        </div>
      </div>

      <div class="input-group">
        <label for="password">Senha</label>
        <div class="input-wrapper">
          <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect width="18" height="11" x="3" y="11" rx="2" ry="2"/><path d="M7 11V7a5 5 0 0 1 10 0v4"/></svg>
          <input 
            type="password" 
            id="password" 
            v-model="formData.password" 
            @input="clearErrors"
            :placeholder="isLoginMode ? 'Sua senha' : 'Mínimo de 8 caracteres'"
            required 
          />
        </div>
      </div>

      <div v-if="!isLoginMode" class="input-group">
        <label for="confirmPassword">Confirmar Senha</label>
        <div class="input-wrapper">
          <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect width="18" height="11" x="3" y="11" rx="2" ry="2"/><path d="M7 11V7a5 5 0 0 1 10 0v4"/></svg>
          <input 
            type="password" 
            id="confirmPassword" 
            v-model="formData.confirmPassword" 
            @input="clearErrors"
            placeholder="Repita a senha" 
            required 
          />
        </div>
      </div>

      <span v-if="errorMessage" class="error-message">{{ errorMessage }}</span>

      <button type="submit" :disabled="!isFormValid" class="submit-btn">
        {{ isLoginMode ? 'Entrar' : 'Criar Conta' }}
      </button>
    </form>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue';

const props = defineProps({
  mode: {
    type: String,
    default: 'register'
  }
});

const emit = defineEmits(['submit']);

const formData = ref({
  email: '',
  password: '',
  confirmPassword: ''
});

const errorMessage = ref('');

const isLoginMode = computed(() => props.mode === 'login');

// Validação estrita no client-side antes de habilitar submissão
const isFormValid = computed(() => {
  if (isLoginMode.value) {
    return formData.value.email.includes('@') && formData.value.password.length > 0;
  }

  return formData.value.email.includes('@') &&
         formData.value.password.length >= 8 &&
         formData.value.password === formData.value.confirmPassword;
});

watch(() => props.mode, () => {
  formData.value.password = '';
  formData.value.confirmPassword = '';
  errorMessage.value = '';
});

const clearErrors = () => {
  errorMessage.value = '';
};

const handleSubmit = () => {
  if (isLoginMode.value) {
    emit('submit', {
      email: formData.value.email,
      password: formData.value.password
    });
    return;
  }

  if (formData.value.password !== formData.value.confirmPassword) {
    errorMessage.value = 'As senhas não coincidem.';
    return;
  }
  
  if (formData.value.password.length < 8) {
    errorMessage.value = 'A senha deve ter no mínimo 8 caracteres.';
    return;
  }

  // Emite o payload limpo e validado para a View
  emit('submit', {
    email: formData.value.email,
    password: formData.value.password
  });
};
</script>

<style scoped>
.form-container {
  background: rgba(30, 27, 75, 0.4);
  backdrop-filter: blur(12px);
  border: 1px solid rgba(139, 92, 246, 0.3);
  border-radius: 12px;
  padding: 2.5rem;
  width: 100%;
  max-width: 450px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.5), 0 0 15px rgba(139, 92, 246, 0.15);
  color: #fff;
  font-family: 'Inter', sans-serif; /* Assumindo a fonte do seu projeto */
}

.form-header {
  text-align: center;
  margin-bottom: 2rem;
}

.form-header h2 {
  margin: 0;
  font-size: 1.5rem;
  font-weight: 600;
  letter-spacing: 0.5px;
}

.form-header p {
  margin-top: 0.5rem;
  font-size: 0.9rem;
  color: #a78bfa;
}

.input-group {
  margin-bottom: 1.5rem;
  display: flex;
  flex-direction: column;
}

.input-group label {
  font-size: 0.85rem;
  font-weight: 500;
  margin-bottom: 0.5rem;
  color: #e2e8f0;
}

.input-wrapper {
  display: flex;
  align-items: center;
  border-bottom: 1px solid rgba(139, 92, 246, 0.4);
  padding-bottom: 0.5rem;
  transition: border-color 0.3s ease;
}

.input-wrapper:focus-within {
  border-bottom-color: #a78bfa;
}

.input-wrapper svg {
  color: #a78bfa;
  margin-right: 10px;
}

.input-wrapper input {
  background: transparent;
  border: none;
  color: #fff;
  width: 100%;
  font-size: 0.95rem;
  outline: none;
}

.input-wrapper input::placeholder {
  color: rgba(255, 255, 255, 0.3);
}

.error-message {
  color: #ef4444;
  font-size: 0.85rem;
  display: block;
  margin-bottom: 1rem;
  text-align: center;
}

.submit-btn {
  width: 100%;
  padding: 0.8rem;
  background: #6d28d9;
  color: #fff;
  border: none;
  border-radius: 6px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.3s ease, transform 0.1s ease;
  margin-top: 1rem;
}

.submit-btn:hover:not(:disabled) {
  background: #7c3aed;
}

.submit-btn:active:not(:disabled) {
  transform: scale(0.98);
}

.submit-btn:disabled {
  background: rgba(109, 40, 217, 0.4);
  cursor: not-allowed;
  color: rgba(255, 255, 255, 0.5);
}
</style>