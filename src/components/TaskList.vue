<!-- mostra totes les tasques-->

<script>

</script>

<template>
   <!-- Lista de tareas -->
<ul>
    <li
    v-for="(task, index) in displayedTasks"
    :key="index"
    :class="{ done: task.done }"
    >
    <input type="checkbox" v-model="task.done" />

    <div class="task-info">
        <span class="task-text">{{ task.text }}</span>
        <small class="task-date">{{ task.date }}</small>
    </div>

    <button class="delete" @click="deleteTask(index)">🗑️</button>
    </li>
</ul>
</template>