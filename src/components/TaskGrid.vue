<template>
  <div class="task-grid">
    <template v-if="tasks.length">
        <!-- :task="task" Pega cada uma das task do v-for e passa 'task' como objeto -->
        <Task v-for="(task, id) in tasks" :key="task.name" :task="task" 
        @taskDeleted="$emit('taskDeleted', id)"
        @taskStateChanged="$emit('taskStateChanged', id)"></Task>
    </template>
    <p v-else class="no-task">Nenhuma tarefa disponível.</p>
  </div>
</template>

<script>
import Task from './Task.vue'

export default {
    components: {
        Task
    },
    props: {
        tasks: { type: Array, required: true}
    }
}
</script>

<style>
    .task-grid {
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
    }

    .task-grid .task{
        margin: 10px;
    }
</style>