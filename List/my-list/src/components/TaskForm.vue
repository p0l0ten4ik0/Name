<template>
  <div class="task-card">
    <h3>{{ task.title }}</h3>
    <p>{{ task.text }}</p>
  </div>
</template>

<script setup lang="ts">
import type { ITask } from './Class'
defineProps<{ task: ITask }>()
</script>

<style scoped>
.task-card {
  border: 1px solid #ffffff;
  border-radius: 8px;
  padding: 12px;
  background-color: #f9f9f9;
}
</style>   