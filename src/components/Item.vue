<script setup>
import { ref, nextTick } from 'vue'

const props = defineProps({
  todo: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['toggle-task', 'delete-task', 'edit-task'])

const isEditing = ref(false)
const editedTitle = ref('')
const inputRef = ref(null)

function startEditing() {
  isEditing.value = true
  editedTitle.value = props.todo.title

  nextTick(() => {
    inputRef.value?.focus()
  })
}

function saveEdit() {
  if (!isEditing.value) return

  const newTitle = editedTitle.value.trim()

  if (!newTitle) {
    cancelEdit()
    return
  }

  emit('edit-task', {
    id: props.todo.id,
    newTitle
  })

  isEditing.value = false
}

function cancelEdit() {
  isEditing.value = false
  editedTitle.value = props.todo.title
}
</script>

<template>
  <li class="todo-item" :class="{ 'is-completed': todo.completed }">
    <template v-if="!isEditing">
      <label class="todo-label">
        <input type="checkbox" :checked="todo.completed" @change="emit('toggle-task', todo.id)" />

        <span class="todo-title" @dblclick="startEditing" title="Klik dua kali untuk edit">
          {{ todo.title }}
        </span>
      </label>

      <button class="delete-button" type="button" @click="emit('delete-task', todo.id)">
        Hapus
      </button>
    </template>

    <input
      v-else
      ref="inputRef"
      v-model="editedTitle"
      class="edit-input"
      type="text"
      aria-label="Edit tugas"
      @keydown.enter="saveEdit"
      @keydown.esc="cancelEdit"
      @blur="saveEdit"
    />
  </li>
</template>

<style scoped>
.todo-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  min-height: 52px;
  padding: 8px 9px 8px 12px;
  border-bottom: 1px solid #ece5d9;
  background: transparent;
  transition: background-color 140ms ease;
}

.todo-item:hover {
  background: #faf6ed;
}

.todo-label {
  display: flex;
  align-items: center;
  gap: 10px;
  min-width: 0;
  cursor: pointer;
}

.todo-label input {
  width: 16px;
  height: 16px;
  flex: 0 0 auto;
  accent-color: #a83d32;
  cursor: pointer;
}

.todo-title {
  overflow-wrap: anywhere;
  color: #38352e;
  font-size: 0.95rem;
  line-height: 1.5;
}

.is-completed .todo-title {
  color: #928a7d;
  text-decoration: line-through;
}

.delete-button {
  flex: 0 0 auto;
  padding: 6px 8px;
  border: 0;
  border-radius: 2px;
  background: transparent;
  color: #8b8173;
  cursor: pointer;
  font-size: 0.82rem;
  font-weight: 600;
}

.delete-button:hover {
  background: #f8e9e4;
  color: #a83d32;
}

.delete-button:focus-visible,
.todo-label input:focus-visible {
  outline: 2px solid #c66d5e;
  outline-offset: 2px;
}

.edit-input {
  width: 100%;
  min-height: 38px;
  padding: 6px 9px;
  border: 1px solid #b84539;
  border-radius: 3px;
  outline: 2px solid rgb(184 69 57 / 12%);
}
</style>
