<script setup lang="ts">
import { ref, onMounted, onUnmounted } from "vue";

const apiUrl = "https://jsonplaceholder.typicode.com/posts/1";
const messageAPI = ref("");
const displayedText = ref("");
const isLoading = ref(true);
const error = ref("");

async function consumeAPI() {
  try {
    isLoading.value = true;
    error.value = "";
    displayedText.value = "";

    const response = await fetch(apiUrl);
    const data = await response.json();

    messageAPI.value = data.title;

    return messageAPI.value;
  } catch (err: Error | unknown) {
    console.log(err);
    error.value = "Erro ao consumir a API: " + (err as Error).message;
  } finally {
    isLoading.value = false;
  }
}

onMounted(() => {
  consumeAPI();
});
</script>

<template>
  <div v-if="isLoading" class="loading">
    <span class="block">
      <div class="spinner"></div>
      Carregando mensagem...
    </span>
  </div>

  <div v-else-if="error" class="error">
    {{ error }}
  </div>

  <div v-else class="success">
    <h3>Olá Mundo, {{ messageAPI }}</h3>
  </div>
</template>

<style scoped>
* {
  height: 99vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(135deg, #f5f5f5ee 40%, #ffffff 60%);
}

.block {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
}

.loading {
  flex-direction: column;
  align-items: center;
  color: #666;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #e0e0e0;
  border-top: 4px solid #666;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.success {
  animation: fadeIn 0.5s ease-in;
}

.cursor {
  animation: blink 1s infinite;
  color: #252725;
  font-weight: bold;
}
</style>
