<!-- mostra totes les tasques-->

<template>
  <ul>
    <TaskItem
      v-for="(task, index) in tasks"
      :key="index"
      :task="task"
      :index="index"
      @delete="deleteTask" 
      @toggle="toggleDone"
    />
  </ul>
<!--@delete lo que hace es escuchar el evento-->
</template>

<script setup>
import TaskItem from './TaskItem.vue'
import { defineProps, defineEmits } from 'vue'

//defineProps permite que el componente reciba datos padre 
const props = defineProps({ //Task(objeto de la tarea) e index(posición de la tarea e el array ) 
  tasks: Array
})
//Permite que en los componentes emitan eventos hacia asus padres 
const emit = defineEmits(['delete', 'toggle'])

const deleteTask = (index) => emit('delete', index)
const toggleDone = (index) => emit('toggle', index)
</script>

