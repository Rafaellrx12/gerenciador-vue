<template>
  <div class="min-h-screen bg-gray-100 py-12 px-4 sm:px-6 lg:px-8">
    <div class="max-w-3xl mx-auto">
      <Header />

      <!-- Adicionando tarefa direto sem modal -->
      <AddTaskButton @addTask="addNewTask" />

      <div class="mb-8">
        <h2 class="text-2xl font-bold text-gray-900 mb-4">Active Tasks</h2>
        <TaskList
          :tasks="activeTasks"
          @toggleTaskCompletion="toggleTaskCompletion"
          @deleteTask="deleteTask"
        />
      </div>

      <div class="mb-8">
        <h2 class="text-2xl font-bold text-gray-900 mb-4">Completed Tasks</h2>
        <TaskList
          :tasks="completedTasks"
          @toggleTaskCompletion="toggleTaskCompletion"
          @deleteTask="deleteTask"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import Header from './Header.vue'
import AddTaskButton from './AddTaskButton.vue'
import TaskList from './TaskList.vue'
import axios from 'axios'

const tasks = ref([])

const apiBaseUrl = import.meta.env.VITE_API_BASE_URL

// Função para carregar as tarefas
const loadTasks = async () => {
  try {
    const response = await axios.get(`${apiBaseUrl}/tasks`)
    tasks.value = response.data
  } catch (error) {
    console.error('Error loading tasks:', error)
  }
}

const activeTasks = computed(() => tasks.value.filter(task => !task.isCompleted))
const completedTasks = computed(() => tasks.value.filter(task => task.isCompleted))

// Função para adicionar nova tarefa
const addNewTask = async (taskDescription) => {
  if (taskDescription) {
    try {
      const response = await axios.post(`${apiBaseUrl}/tasks`, {
        description: taskDescription,
        isCompleted: false
      })
      tasks.value.push(response.data)
      loadTasks()
    } catch (error) {
      console.error('Error adding task:', error)
    }
  }
}

const deleteTask = async (id) => {
  try {
    await axios.delete(`${apiBaseUrl}/tasks/${id}`)
    tasks.value = tasks.value.filter(task => task.id !== id)
  } catch (error) {
    console.error('Error deleting task:', error)
  }
}

const toggleTaskCompletion = async (task) => {
  try {
    const updatedTask = { isCompleted: !task.isCompleted }
    await axios.patch(`${apiBaseUrl}/tasks/${task.id}`, updatedTask)
    task.isCompleted = !task.isCompleted
    loadTasks()
  } catch (error) {
    console.error('Error updating task:', error)
  }
}

// Carregar as tarefas quando o componente é montado
loadTasks()
</script>
