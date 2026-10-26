<script setup>
import { ref } from 'vue'

const emit = defineEmits(['add-task'])

const task = ref('')
const errorMessage = ref('')

function addTask() {
  const title = task.value.trim()

  if (!title) {
    errorMessage.value = 'Tugas tidak boleh kosong!'
    return
  }

  emit('add-task', title)
  task.value = ''
  errorMessage.value = ''
}

function clearError() {
  errorMessage.value = ''
}
</script>

<template>
  <div class="task-entry">
    <form class="task-form" @submit.prevent="addTask">
      <input
        v-model="task"
        type="text"
        placeholder="Tulis tugas baru..."
        aria-label="Tugas baru"
        @input="clearError"
      />

      <button type="submit">Tambah tugas</button>
    </form>

    <p v-if="errorMessage" class="input-error" role="alert">
      {{ errorMessage }}
    </p>
  </div>
</template>

<style scoped>
.task-form {
  display: flex;
  gap: 8px;
}

input {
  width: 100%;
  min-width: 0;
  min-height: 48px;
  padding: 0 13px;
  border: 1px solid #ddd5c8;
  border-radius: 3px;
  background: #fffefa;
  color: #292722;
  outline: none;
  transition: border-color 140ms ease, background-color 140ms ease;
}

input::placeholder {
  color: #9a9285;
}

input:focus {
  border-color: #b84539;
  background: #fff;
  box-shadow: 3px 3px 0 rgb(184 69 57 / 10%);
}

button {
  flex: 0 0 auto;
  min-height: 48px;
  padding: 0 17px;
  border: 0;
  border-radius: 3px;
  background: #a83d32;
  color: #fff;
  cursor: pointer;
  font-size: 0.9rem;
  font-weight: 650;
  transition: background-color 140ms ease, transform 140ms ease;
}

button:hover {
  background: #873027;
}

button:active {
  transform: translateY(1px);
}

button:focus-visible {
  outline: 2px solid #c66d5e;
  outline-offset: 2px;
}

.input-error {
  margin: 8px 2px 0;
  color: #a83d32;
  font-size: 0.875rem;
}

@media (max-width: 420px) {
  .task-form {
    flex-direction: column;
  }
}
</style>