<!--gestiona l’entrada d’una nova tasca.-->

<template>

    <!-- Formulario para agregar tareas -->
    <div class="form">
      <input
        v-model="newTask"
        type="text"
        placeholder="Escribe una tarea..."
        @keyup.enter="addTask"
      />
      <input
        v-model="newDate"
        type="date"
        class="date-input"
      />
      <button @click="addTask">Agregar</button>
    </div>
</template>

<script>
import { ref } from 'vue'

const newTask = ref('')
const newDate = ref('')
const tasks = ref([])

defineProps([tasks])  

// Agregar tarea
const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push({
      text: newTask.value.trim(),
      done: false,
      date: newDate.value || new Date().toISOString().slice(0, 10)
    })
    
    newTask.value = ''
    newDate.value = ''
  }
}
</script>