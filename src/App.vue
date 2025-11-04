<template>
  <!-- Contenedor principal -->
  <div class="container">
    <h1> To-Do List</h1>

    <!-- Formulario para escribir una nueva tarea -->
    <div class="form">
      <!-- Input que se conecta a la variable newTask -->
      <!-- v-model Enlaza el texto al dato 'newTask' -->
      <!-- keyup.enter Si presionas Enter, se ejecuta addTask() -->
      <input
        v-model="newTask"               
        type="text"
        placeholder="Escribe una tarea..."
        @keyup.enter="addTask"          
      />
      <input 
      v-model="newDate"
      type="date"
      class="date-input">

      <button @click="addTask">Agregar</button> <!-- Botón que llama a addTask() -->
    </div>

    <!-- Lista de tareas -->
    <ul>
      <!-- Recorre el arreglo 'tasks' y muestra cada tarea -->
      <!-- Recorre con v-for -->
      <!-- Clave única (requerida por Vue) -->
      <!-- Si la tarea está hecha, aplica la clase 'done' -->
       <li
    v-for="(task, index) in sortedTasks"
    :key="index"
    :class="{ done: task.done }"
  >
    <input type="checkbox" v-model="task.done" />

    <div class="task-info">
      <span class="task-text">{{ task.text }}</span>
      <small class="task-date"> {{ task.date }}</small>
    </div>

    <button class="delete" @click="deleteTask(index)">🗑️</button>
  </li>
    </ul>

    <!-- Mensaje si no hay tareas -->
    <p v-if="!tasks.length">No tienes tareas pendientes </p>
  </div>
</template>

<script setup>
// Importamos las funciones reactivas de Vue
import { ref ,computed} from 'vue'

// --------------------- VARIABLES REACTIVAS ---------------------

// Texto actual del input
const newTask = ref('')

//Fecha
const newDate = ref('')

// Lista de tareas (cada tarea es un objeto con { text, done })
const tasks = ref([])


// --------------------- FUNCIONES PRINCIPALES ---------------------

// Agregar una nueva tarea
const addTask = () => {
  // Si el input no está vacío...
  if (newTask.value.trim() !== '') {
    // Agregamos la nueva tarea al array
    tasks.value.push({
      text: newTask.value.trim(), // el texto escrito
      done: false,                 // por defecto, no hecha
      date: newDate.value || new Date().toISOString().slice(0,10)
    })
    // Limpiamos el input
    newTask.value = ''
    newDate.value = ''
  }
}

// Eliminar una tarea del array
const deleteTask = (index) => {
  tasks.value.splice(index, 1) // elimina la tarea en esa posición
}

//Orden de las fechas 
const sortedTasks = computed(()=> {
  return [...tasks.value].sort((a,b) => new Date(a.date) - new Date(b.date))
})

</script>


<style scoped>
/* Contenedor principal */
.container {
  max-width: 420px;
  margin: 50px auto;
  background: #ffffff;
  padding: 25px 30px;
  border-radius: 15px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.08);
  text-align: center;
  font-family: "Poppins", sans-serif;
  color: #333;
  transition: all 0.3s ease;
}

/* Título principal */
h1 {
  color: #42b983;
  font-size: 1.8rem;
  margin-bottom: 25px;
  letter-spacing: 1px;
}

/* Formulario de nueva tarea */

.form {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 10px;
  margin-bottom: 20px;
}

.date-input {
  padding: 8px;
  border-radius: 5px;
  border: 1px solid #ccc;
  font-size: 14px;
  color: #555;
}

.task-info {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  flex: 1;
}

.task-text {
  font-size: 15px;
  margin-bottom: 3px;
}

.task-date {
  font-size: 12px;
  color: #666;
}

.done .task-text {
  text-decoration: line-through;
  color: gray;
}


input[type="text"] {
  flex: 1;
  padding: 10px 15px;
  font-size: 16px;
  border-radius: 8px;
  border: 1.5px solid #ddd;
  outline: none;
  transition: border-color 0.3s ease;
}

input[type="text"]:focus {
  border-color: #42b983;
  box-shadow: 0 0 6px rgba(66, 185, 131, 0.4);
}

/* Botón “Agregar” */
button {
  padding: 10px 15px;
  background: #42b983;
  color: white;
  font-weight: 600;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background 0.3s ease, transform 0.2s ease;
}

button:hover {
  background: #3aa874;
  transform: scale(1.05);
}

/* Botón eliminar */
button.delete {
  background: #e74c3c;
  font-size: 14px;
  padding: 8px 10px;
  border-radius: 6px;
  transition: background 0.3s ease, transform 0.2s ease;
}

button.delete:hover {
  background: #c0392b;
  transform: scale(1.05);
}

/* Lista de tareas */
ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: #f9f9f9;
  border-radius: 10px;
  padding: 10px 14px;
  margin-bottom: 10px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

li:hover {
  transform: translateY(-2px);
  box-shadow: 0 3px 10px rgba(0, 0, 0, 0.08);
}

/* Casilla de verificación */
input[type="checkbox"] {
  margin-right: 10px;
  transform: scale(1.3);
  accent-color: #42b983; /* Color verde */
  cursor: pointer;
}

/* Texto de tarea */
span {
  flex: 1;
  text-align: left;
  font-size: 15px;
}

/* Tareas completadas */
.done span {
  text-decoration: line-through;
  color: #9b9b9b;
}

/* Mensaje sin tareas */
p {
  color: #999;
  font-style: italic;
  margin-top: 20px;
}


</style>
