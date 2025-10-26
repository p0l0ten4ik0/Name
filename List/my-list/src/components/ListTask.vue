<template>
  <div class="tasks">
    <TaskItem
      v-for="task in tasks"
      :key="task.id"
      :task="task"
      @delete="onDelete"
      @update="onUpdate"
    />
  </div>
</template>

<script setup lang="ts">
import type { ITask } from './Class'
import TaskItem from './TaskForm.vue'

defineProps<{ tasks: ITask[] }>()
const emit = defineEmits<{ (e: 'delete', id: string): void; (e: 'update', task: ITask): void }>()

function onDelete(id: string) {
  emit('delete', id)
}

function onUpdate(task: ITask) {
  emit('update', task)
}
</script>

<style scoped>
.tasks {
  display: flex;
  flex-direction: column;
  gap: 12px;
  color: #000000;
}
</style>