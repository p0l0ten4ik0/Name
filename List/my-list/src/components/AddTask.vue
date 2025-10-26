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
  gap: 12px;
  margin-bottom: 18px;
  align-items: center;
}
.add-task input {
  flex: 1 1 auto;
  padding: 10px 12px;
  border-radius: 8px;
  border: 1px solid rgba(255,255,255,0.06);
  background: rgba(255,255,255,0.02);
  color: #e6eef8;
}
button {
  padding: 10px 14px;
  background: linear-gradient(180deg,#10b981,#059669);
  border: none;
  color: white;
  border-radius: 8px;
  cursor: pointer;
  box-shadow: 0 6px 18px rgba(16,185,129,0.18);
}

button:hover { transform: translateY(-2px); }
</style>