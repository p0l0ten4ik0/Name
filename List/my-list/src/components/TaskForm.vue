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

// keep local edits in sync when prop changes externally
watch(() => props.task, (t) => {
  editTitle.value = t.title
  editText.value = t.text
})

function startEdit() {
  isEditing.value = true
}

function cancelEdit() {
  // reset local values and close editor
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
  border: 1px solid #ffffff;
  border-radius: 8px;
  padding: 12px;
  background-color: #f9f9f9;
}

.actions {
  margin-top: 8px;
  display: flex;
  gap: 8px;
}

.actions .delete {
  background: #e04b4b;
  color: #fff;
  border: none;
  padding: 6px 10px;
  border-radius: 4px;
}

.edit-form input {
  display: block;
  width: 100%;
  margin-bottom: 8px;
  padding: 6px;
  border-radius: 4px;
  border: 1px solid #ddd;
}
</style>   