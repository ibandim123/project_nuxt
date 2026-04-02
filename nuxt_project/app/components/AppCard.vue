<script setup lang="ts">
type data = {
  userId: number;
  id: number;
  title: string;
  body: string;
};

const {
  data: data,
  error: fetchError,
  pending,
} = await useFetch("https://jsonplaceholder.typicode.com/posts/1");

console.log(pending.value);
const message = computed(
  () => data.value?.title || "Nenhuma mensagem encontrada",
);
const isLoading = computed(() => pending.value);
const hasError = computed(() => !!fetchError.value);
</script>

<template>
  <body class="container">
    <div v-if="isLoading" class="loading">
      <div class="spinner"></div>
      <span>Carregando mensagem...</span>
    </div>

    <div v-else-if="hasError" class="error">
      <p>❌ Ops! Algo deu errado.</p>
      <small>{{ fetchError?.message || "Erro desconhecido" }}</small>
    </div>

    <div v-else-if="message" class="success">
      <h3 aria-live="polite">Olá Mundo, {{ message }}</h3>
    </div>

    <div v-else class="empty">
      <p>📭 Nenhuma mensagem encontrada</p>
    </div>
  </body>
</template>

<style scoped>
.container {
  min-height: 70vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(135deg, #f8f9fa 0%, #ffffff 100%);
  padding: 2rem;
}

.loading {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
  color: #6c757d;
  font-size: 1.1rem;
}

.spinner {
  width: 48px;
  height: 48px;
  border: 3px solid #e9ecef;
  border-top: 3px solid #007bff;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  will-change: transform;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

.error {
  background: #f8d7da;
  color: #721c24;
  padding: 1.5rem;
  border-radius: 8px;
  border-left: 4px solid #dc3545;
  max-width: 400px;
  text-align: center;
}

.success {
  animation: fadeInUp 0.6s ease-out;
  text-align: center;
}

.success h3 {
  font-size: 1.5rem;
  margin: 0;
  font-weight: 600;
}

.cursor {
  animation: blink 1s infinite;
  color: #28a745;
  font-weight: bold;
}

.empty {
  color: #6c757d;
  text-align: center;
  font-size: 1.1rem;
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes blink {
  0%,
  50% {
    opacity: 1;
  }
  51%,
  100% {
    opacity: 0;
  }
}

@media (max-width: 768px) {
  .container {
    padding: 1rem;
  }

  .success h3 {
    font-size: 1.2rem;
  }

  .error {
    padding: 1rem;
  }
}
</style>
