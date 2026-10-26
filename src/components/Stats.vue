<script setup>
import { computed } from 'vue'

const props = defineProps({
  remainingTasks: {
    type: Number,
    required: true
  },
  totalTasks: {
    type: Number,
    required: true
  }
})

const completedTasks = computed(() => {
  return props.totalTasks - props.remainingTasks
})

const completionPercentage = computed(() => {
  if (props.totalTasks === 0) return 0

  return Math.round(
    (completedTasks.value / props.totalTasks) * 100
  )
})
</script>

<template>
  <div class="stats">
    <div class="stats-grid">
      <div class="stat-card">
        <span>Total tugas</span>
        <strong>{{ totalTasks }}</strong>
      </div>

      <div class="stat-card">
        <span>Selesai</span>
        <strong>{{ completedTasks }}</strong>
      </div>

      <div class="stat-card">
        <span>Tersisa</span>
        <strong>{{ remainingTasks }}</strong>
      </div>
    </div>

    <div v-if="totalTasks > 0" class="progress-section">
      <div class="progress-label">
        <span>Penyelesaian tugas</span>
        <strong>{{ completionPercentage }}%</strong>
      </div>

      <div
        class="progress-track"
        role="progressbar"
        :aria-valuenow="completionPercentage"
        aria-valuemin="0"
        aria-valuemax="100"
        :aria-label="`Penyelesaian tugas ${completionPercentage}%`"
      >
        <div class="progress-fill" :style="{ width: completionPercentage + '%' }"></div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.stats {
  margin-top: 22px;
  padding-top: 18px;
  border-top: 1px solid #ded6c9;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 0;
}

.stat-card {
  display: flex;
  flex-direction: column;
  gap: 5px;
  padding: 5px 12px;
  border-left: 1px solid #e5ddd0;
}

.stat-card:first-child {
  padding-left: 0;
  border-left: 0;
}

.stat-card span {
  color: #827a6e;
  font-family: ui-monospace, "SFMono-Regular", Consolas, monospace;
  font-size: 0.68rem;
  letter-spacing: 0.035em;
}

.stat-card strong {
  color: #302d27;
  font-family: Georgia, "Times New Roman", serif;
  font-size: 1.35rem;
  font-weight: 500;
}

.progress-section {
  margin-top: 20px;
}

.progress-label {
  display: flex;
  justify-content: space-between;
  gap: 12px;
  margin-bottom: 8px;
  color: #777166;
  font-size: 0.8rem;
}

.progress-label strong {
  color: #a83d32;
  font-family: ui-monospace, "SFMono-Regular", Consolas, monospace;
}

.progress-track {
  overflow: hidden;
  height: 6px;
  border-radius: 1px;
  background: #e8e0d4;
}

.progress-fill {
  height: 100%;
  border-radius: inherit;
  background: #b84539;
  transition: width 180ms ease;
}

@media (max-width: 400px) {
  .stats-grid {
    gap: 7px;
  }

  .stat-card {
    padding: 5px 8px;
  }

  .stat-card span {
    font-size: 0.64rem;
  }
}
</style>