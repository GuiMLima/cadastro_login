<template>
  <main class="dashboard-page">
    <h1>Dashboard</h1>
    <p>Cadastro concluido com sucesso.</p>
  </main>
</template>

<style scoped>
.dashboard-page {
  min-height: 100vh;
  width: min(96vw, 1600px);
  margin: 0 auto;
  display: grid;
  place-content: center;
  gap: 0.75rem;
  text-align: center;
  background: #0f172a;
  color: #f8fafc;
  padding: clamp(1rem, 3vw, 2.5rem);
}

h1 {
  font-size: clamp(1.8rem, 3vw, 2.4rem);
}

p {
  color: #cbd5e1;
}
</style>
