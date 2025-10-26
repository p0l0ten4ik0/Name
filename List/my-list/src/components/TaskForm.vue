<template>
  <div class="task-card">
    <div v-if="!isEditing">
      <h3>{{ task.title }}</h3>
      <p>{{ task.text }}</p>
      <div class="actions">
        <button type="button" @click="startEdit">Редактировать</button>
        <button type="button" class="delete" @click="onDelete">Удалить</button>
      </div>
    </div>

    <form v-else @submit.prevent="onSave" class="edit-form">
      <input v-model="editTitle" type="text" required />
      <input v-model="editText" type="text" required />
      <div class="actions">
        <button type="submit">Сохранить</button>
        <button type="button" @click="cancelEdit">Отмена</button>
      </div>
    </form>
  </div>
</template>

<script setup lang="ts">
import type { ITask } from './Class'
const props = defineProps<{ task: ITask }>()
const emit = defineEmits<{ (e: 'delete', id: string): void; (e: 'update', task: ITask): void }>()

import { ref, watch } from 'vue'

const isEditing = ref(false)
const editTitle = ref(props.task.title)
const editText = ref(props.task.text)

watch(() => props.task, (t) => {
  editTitle.value = t.title
  editText.value = t.text
})

function startEdit() {
  isEditing.value = true
}

function cancelEdit() {
  editTitle.value = props.task.title
  editText.value = props.task.text
  isEditing.value = false
}

function onSave() {
  const updated: ITask = {
    id: props.task.id,
    title: editTitle.value,
    text: editText.value,
  }
  emit('update', updated)
  isEditing.value = false
}

function onDelete() {
  emit('delete', props.task.id)
}
</script>

<style scoped>
.task-card {
  border-radius: 10px;
  padding: 14px;
  background: linear-gradient(180deg, #0b1320, #071025);
  border: 1px solid rgba(255,255,255,0.04);
  color: #e6eef8;
  box-shadow: 0 6px 18px rgba(2,6,23,0.6);
}

.actions {
  margin-top: 10px;
  display: flex;
  gap: 8px;
}

.actions .delete {
  background: linear-gradient(180deg,#ef4444,#d03b3b);
  color: #fff;
  border: none;
  padding: 8px 12px;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(220,38,38,0.2);
}

.actions button {
  background: linear-gradient(180deg,#111827,#0b1220);
  color: #e6eef8;
  border: 1px solid rgba(255,255,255,0.04);
}

.edit-form input {
  display: block;
  width: 100%;
  margin-bottom: 8px;
  padding: 10px 12px;
  border-radius: 8px;
  border: 1px solid rgba(255,255,255,0.06);
  background: rgba(255,255,255,0.02);
  color: #e6eef8;
}

.edit-form input::placeholder { color: rgba(230,238,248,0.45); }
</style>   