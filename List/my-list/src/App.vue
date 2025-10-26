<template>
  <div class="app">
    <h1>Список дел</h1>

    <AddTaskForm @add="addTask" />
    <TaskList :tasks="tasks" />
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { tasksData } from './components/Task'
import type { ITask } from './components/Class'

import TaskList from './components/ListTask.vue'
import AddTaskForm from './components/AddTask.vue'

const tasks = ref<ITask[]>([...tasksData])

function addTask(newTask: ITask) {
  tasks.value.push(newTask)
}
</script>

<style scoped>
.app {
  padding: 20px;
  font-family: sans-serif;
  max-width: 600px;
  margin: 0 auto;
}
</style>