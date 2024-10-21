<template>
  <div class="flex items-center">
    <input
      v-model="taskDescription"
      type="text"
      placeholder="Adicionar tarefa..."
      class="w-full px-4 py-2 border-b border-gray-300 bg-transparent focus:outline-none focus:border-primary transition"
      @keyup.enter="submitTask"
    />
    <button
      @click="submitTask"
      class="px-4 py-2 bg-gradient-to-r from-cyan-400 to-blue-500 text-white rounded-lg focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-primary transition"
    >
      <PlusIcon class="h-5 w-5" />
    </button>
  </div>
</template>

<script setup>
import { ref, defineEmits } from 'vue'
import { PlusIcon } from 'lucide-vue-next'

// Define os eventos que o componente pode emitir
const emit = defineEmits(['addTask'])

// Estado para a nova tarefa
const taskDescription = ref('')

// Função para submeter a nova tarefa
const submitTask = () => {
  if (taskDescription.value.trim()) {
    // Emitindo o evento para o componente pai
    emit('addTask', taskDescription.value.trim())
    taskDescription.value = ''
  }
}
</script>

<style scoped>
/* Ajustes de estilo para remover as bordas laterais e superiores */
input {
  border-top: none;
  border-left: none;
  border-right: none;
  background-color: transparent;
}
</style>
