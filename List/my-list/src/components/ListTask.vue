<template>
  <div class="tasks">
    <TaskItem v-for="task in tasks" :key="task.id" :task="task" />
  </div>
</template>

<script setup lang="ts">
import type { ITask } from './Class'
import TaskItem from './TaskForm.vue'

defineProps<{ tasks: ITask[] }>()
</script>

<style scoped>
.tasks {
  display: flex;
  flex-direction: column;
  gap: 12px;
  color: #000000;
}
</style>