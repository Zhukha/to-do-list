<script setup>
import { ref, computed, watch, onMounted } from 'vue'
import Input from '@/components/Input.vue'
import Item from '@/components/Item.vue'
import Filter from '@/components/Filter.vue'
import Stats from '@/components/Stats.vue'

const STORAGE_KEY = 'list_to_do'
const todos = ref([])

onMounted(() => {
  const savedTodos = localStorage.getItem(STORAGE_KEY)
  if (savedTodos) {
    try {
      todos.value = JSON.parse(savedTodos)
    } catch {
      todos.value = []
    }
  }
})

watch(todos, (updatedTodos) => {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(updatedTodos))
}, { deep: true })

const activeFilter = ref('all')

const taskList = computed(() => {
  if (activeFilter.value === 'active') {
    return todos.value.filter(todo => !todo.completed)
  }
  if (activeFilter.value === 'completed') {
    return todos.value.filter(todo => todo.completed)
  }
  return todos.value
})

function setActiveFilter(value) {
  activeFilter.value = value
}

const remainingTasks = computed(() => {
  return todos.value.filter(todo => !todo.completed).length
})

function createTask(title) {
  todos.value.push({
    id: Date.now(),
    title,
    completed: false
  })
}

function toggleTask(id) {
  const task = todos.value.find(todo => todo.id === id)
  if (task) {
    task.completed = !task.completed
  }
}

function deleteTask(id) {
  todos.value = todos.value.filter(todo => todo.id !== id)
}

function editTask({ id, newTitle }) {
  const task = todos.value.find(todo => todo.id === id)

  if (task) {
    task.title = newTitle
  }
}
</script>

<template>
  <main class="app-shell">
    <header class="app-header">
      <p class="eyebrow">Ruang kerja pribadi</p>
      <h1>Daftar Tugas</h1>
      <p class="subtitle">Atur hal-hal yang ingin kamu selesaikan hari ini.</p>
    </header>

    <section class="todo-card">
      <Input @add-task="createTask" />

      <Filter :active-filter="activeFilter" @filter-change="setActiveFilter" />

      <ul v-if="taskList.length" class="todo-list">
        <Item
          v-for="todo in taskList"
          :key="todo.id"
          :todo="todo"
          @toggle-task="toggleTask"
          @delete-task="deleteTask"
          @edit-task="editTask"
        />
      </ul>

      <div v-else class="empty-state">
        <p>Tidak ada tugas pada filter ini.</p>
      </div>

      <Stats :remaining-tasks="remainingTasks" :total-tasks="todos.length" />
    </section>
  </main>
</template>

<style scoped>
:global(*) {
  box-sizing: border-box;
}

:global(body) {
  margin: 0;
  min-width: 320px;
  min-height: 100vh;
  background-color: #f4f0e8;
  background-image: radial-gradient(#d9d0c2 0.7px, transparent 0.7px);
  background-size: 18px 18px;
  color: #292722;
  font-family: "Segoe UI", ui-sans-serif, system-ui, sans-serif;
}

:global(button),
:global(input) {
  font: inherit;
}

.app-shell {
  width: min(100% - 36px, 660px);
  margin: 0 auto;
  padding: 76px 0;
}

.app-header {
  margin-bottom: 26px;
  padding-left: 5px;
}

.eyebrow {
  margin: 0 0 10px;
  color: #a83d32;
  font-family: ui-monospace, "SFMono-Regular", Consolas, monospace;
  font-size: 0.72rem;
  font-weight: 650;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

h1 {
  margin: 0;
  color: #292722;
  font-family: Georgia, "Times New Roman", serif;
  font-size: clamp(2.35rem, 7vw, 3.25rem);
  font-weight: 500;
  letter-spacing: -0.055em;
  line-height: 1.04;
}

.subtitle {
  margin: 12px 0 0;
  color: #777166;
  font-size: 0.96rem;
  line-height: 1.55;
}

.todo-card {
  position: relative;
  padding: 30px;
  border: 1px solid #e2dbcf;
  border-radius: 4px 4px 14px 4px;
  background: #fffdf8;
  box-shadow: 5px 6px 0 #e8e0d4, 0 18px 46px rgb(51 42 30 / 8%);
}

.todo-card::before {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  width: 3px;
  background: #b84539;
  content: "";
}

.todo-list {
  display: grid;
  gap: 8px;
  margin: 21px 0 24px;
  padding: 0;
  list-style: none;
}

.empty-state {
  margin: 20px 0 22px;
  padding: 22px 12px;
  border-top: 1px dashed #ded6c9;
  border-bottom: 1px dashed #ded6c9;
  color: #777166;
  text-align: left;
}

.empty-state p {
  margin: 0;
  font-family: Georgia, "Times New Roman", serif;
  font-size: 1.05rem;
}

@media (max-width: 520px) {
  .app-shell {
    padding: 42px 0;
  }

  .todo-card {
    padding: 23px 18px;
    box-shadow: 3px 4px 0 #e8e0d4, 0 12px 30px rgb(51 42 30 / 7%);
  }
}
</style>