<template>
  <div class="app">
    <h1>Список дел</h1>

    <AddTaskForm @add="addTask" />
    <TaskList :tasks="tasks" @delete="deleteTask" @update="updateTask" />
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { tasksData } from './components/Task'
import type { ITask } from './components/Class'

import TaskList from './components/ListTask.vue'
import AddTaskForm from './components/AddTask.vue'

import { onMounted, watch } from 'vue'

const STORAGE_KEY = 'my-list-tasks'

const tasks = ref<ITask[]>([])

// load saved tasks or fallback to initial data
onMounted(() => {
  try {
    const raw = localStorage.getItem(STORAGE_KEY)
    if (raw) {
      tasks.value = JSON.parse(raw) as ITask[]
      return
    }
  } catch (e) {
    // ignore
  }
  tasks.value = [...tasksData]
})

watch(tasks, (val) => {
  try {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(val))
  } catch (e) {}
}, { deep: true })

function addTask(newTask: ITask) {
  tasks.value.push(newTask)
}

function deleteTask(id: string) {
  tasks.value = tasks.value.filter(t => t.id !== id)
}

function updateTask(updated: ITask) {
  const idx = tasks.value.findIndex(t => t.id === updated.id)
  if (idx !== -1) {
    tasks.value[idx] = { ...tasks.value[idx], ...updated }
  }
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