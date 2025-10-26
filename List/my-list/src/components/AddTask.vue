<template>
  <form @submit.prevent="addTask" class="add-task">
    <input v-model="title" type="text" placeholder="Название дела" required />
    <input v-model="text" type="text" placeholder="Описание" required />
    <button type="submit">Добавить</button>
  </form>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import type { ITask } from './Class'

const emit = defineEmits<{ add: [task: ITask] }>()

const title = ref('')
const text = ref('')

function addTask() {
  if (!title.value || !text.value) return

  emit('add', {
    id: Date.now().toString(),
    title: title.value,
    text: text.value
  })

  title.value = ''
  text.value = ''
}
</script>

<style scoped>
.add-task {
  display: flex;
  gap: 8px;
  margin-bottom: 16px;
}
.add-task input {
  padding: 6px;
  border-radius: 4px;
  border: 1px solid #f8f7f7;
}
button {
  padding: 6px 12px;
  background-color: #42b983;
  border: none;
  color: white;
  border-radius: 4px;
  cursor: pointer;
}
</style>