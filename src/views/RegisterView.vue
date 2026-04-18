<template>
  <main class="register-page">
    <section class="auth-panel">
      <div class="auth-toggle" role="tablist" aria-label="Tipo de acesso">
        <button
          type="button"
          class="toggle-btn"
          :class="{ active: authMode === 'register' }"
          @click="authMode = 'register'"
        >
          Cadastro
        </button>
        <button
          type="button"
          class="toggle-btn"
          :class="{ active: authMode === 'login' }"
          @click="authMode = 'login'"
        >
          Login
        </button>
      </div>

      <RegisterForm :mode="authMode" @submit="handleAuth" />
    </section>
  </main>
</template>

<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import RegisterForm from '@/components/RegisterForm.vue';

const router = useRouter();
const authMode = ref('register');

const handleAuth = async (credentials) => {
  try {
    console.log('Iniciando requisição HTTP com:', credentials, authMode.value);
    
    await new Promise(resolve => setTimeout(resolve, 1500));

    if (authMode.value === 'login') {
      console.log('Usuário autenticado com sucesso.');
    } else {
      console.log('Usuário registrado com sucesso. UUID gerado no back-end.');
    }
    
    router.push('/dashboard');
    
  } catch (error) {
    console.error('Falha no processo de autenticação:', error);
  }
};
</script>

<style scoped>
.register-page {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: #0f172a;
  padding: 1rem;
}

.auth-panel {
  width: 100%;
  max-width: 450px;
}

.auth-toggle {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0.35rem;
  margin-bottom: 1rem;
  padding: 0.35rem;
  border-radius: 999px;
  border: 1px solid rgba(139, 92, 246, 0.3);
  background: rgba(30, 27, 75, 0.6);
  backdrop-filter: blur(12px);
}

.toggle-btn {
  border: 0;
  border-radius: 999px;
  padding: 0.55rem 0.8rem;
  color: #ddd6fe;
  background: transparent;
  font-size: 0.9rem;
  font-weight: 600;
  cursor: pointer;
  transition: background-color 0.2s ease, color 0.2s ease;
}

.toggle-btn.active {
  color: #fff;
  background: #6d28d9;
}
</style>