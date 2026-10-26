<script setup>
defineProps({
  activeFilter: {
    type: String,
    required: true
  }
})

const emit = defineEmits(['filter-change'])

const filterOptions = [
  { label: 'Semua', value: 'all' },
  { label: 'Aktif', value: 'active' },
  { label: 'Selesai', value: 'completed' }
]
</script>

<template>
  <div class="filter-bar" role="group" aria-label="Filter tugas">
    <button
      v-for="option in filterOptions"
      :key="option.value"
      type="button"
      :class="{ 'is-active': activeFilter === option.value }"
      :aria-pressed="activeFilter === option.value"
      @click="emit('filter-change', option.value)"
    >
      {{ option.label }}
    </button>
  </div>
</template>

<style scoped>
.filter-bar {
  display: flex;
  gap: 5px;
  margin-top: 20px;
  padding: 4px 0;
  border-bottom: 1px solid #e4ddd1;
}

button {
  flex: 1;
  min-height: 38px;
  padding: 8px 10px;
  border: 0;
  border-radius: 3px 3px 0 0;
  background: transparent;
  color: #817a6e;
  cursor: pointer;
  font-size: 0.86rem;
  font-weight: 600;
  transition: background-color 140ms ease, color 140ms ease;
}

button:hover:not(.is-active) {
  background: #f4eee4;
  color: #3d3932;
}

button.is-active {
  background: #a83d32;
  color: #fff;
}

button:focus-visible {
  outline: 2px solid #c66d5e;
  outline-offset: 2px;
}
</style>