<template>
  <div class="container">
    <h1>To-Do List</h1>

    <TaskForm @add="addTask" />

    <div style="margin-bottom: 10px;">
      <label>
        <input type="checkbox" v-model="pendientes" />
        Mostrar solo tareas pendientes
      </label>
    </div>

    <div style="margin-bottom: 15px;">
      <label>
        <input type="checkbox" v-model="hechas" />
        Mostrar solo tareas completadas
      </label>
    </div>

    <TaskList :tasks="displayedTasks" @delete="deleteTask" @toggle="toggleDone" />

    <p>Total de tareas: {{ tasks.length }}</p>
    <p>Tareas pendientes: {{ pendingTasks.length }}</p>
    <p>Tareas completadas: {{ doneTasks.length }}</p>
    <p v-if="!tasks.length">No tienes tareas pendientes</p>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import TaskForm from './components/TaskForm.vue'
import TaskList from './components/TaskList.vue'

const tasks = ref([])
const pendientes = ref(false)
const hechas = ref(false)

const addTask = (task) => tasks.value.push(task)
const deleteTask = (index) => tasks.value.splice(index, 1)
const toggleDone = (index) => tasks.value[index].done = !tasks.value[index].done

const pendingTasks = computed(() => tasks.value.filter(task => !task.done))
const doneTasks = computed(() => tasks.value.filter(task => task.done))

const displayedTasks = computed(() => {
  let list = [...tasks.value].sort((a, b) => new Date(a.date) - new Date(b.date))
  if (pendientes.value) list = list.filter(task => !task.done)
  if (hechas.value) list = list.filter(task => task.done)
  return list
})
</script>

<style scoped>
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

h1 {
  color: #42b983;
  font-size: 1.8rem;
  margin-bottom: 25px;
  letter-spacing: 1px;
}

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

input[type="checkbox"] {
  margin-right: 10px;
  transform: scale(1.3);
  accent-color: #42b983;
  cursor: pointer;
}

span {
  flex: 1;
  text-align: left;
  font-size: 15px;
}

.done span {
  text-decoration: line-through;
  color: #9b9b9b;
}

p {
  color: #999;
  font-style: italic;
  margin-top: 20px;
}
</style>
