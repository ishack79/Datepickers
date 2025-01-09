<script setup lang="ts">
import { ref, computed } from 'vue'

const props = defineProps<{
  modelValue: {
    fromDate: string
    toDate: string
  }
}>()

const emit = defineEmits<{
  'update:modelValue': [value: { fromDate: string, toDate: string }]
}>()

const fromDate = ref(props.modelValue.fromDate)
const toDate = ref(props.modelValue.toDate)

const handleFromDateChange = (event: Event) => {
  const target = event.target as HTMLInputElement
  fromDate.value = target.value
  // If toDate is before fromDate, reset toDate
  if (toDate.value && toDate.value < fromDate.value) {
    toDate.value = ''
  }
  emit('update:modelValue', { fromDate: fromDate.value, toDate: toDate.value })
}

const handleToDateChange = (event: Event) => {
  const target = event.target as HTMLInputElement
  toDate.value = target.value
  emit('update:modelValue', { fromDate: fromDate.value, toDate: toDate.value })
}

const minToDate = computed(() => fromDate.value || '')
const maxDate = computed(() => {
  const today = new Date()
  return today.toISOString().split('T')[0]
})

const formatDate = (dateString: string) => {
  if (!dateString) return ''
  return new Date(dateString).toLocaleDateString('default', {
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  })
}
</script>

<template>
  <div class="picker-container date-range">
    <h2>Date Range Picker</h2>
    <div class="range-inputs">
      <div class="date-input">
        <label for="fromDate">From:</label>
        <input 
          type="date" 
          id="fromDate"
          :value="fromDate"
          :max="maxDate"
          @change="handleFromDateChange"
          class="date-picker"
        />
      </div>
      <div class="date-input">
        <label for="toDate">To:</label>
        <input 
          type="date" 
          id="toDate"
          :value="toDate"
          :min="minToDate"
          :max="maxDate"
          :disabled="!fromDate"
          @change="handleToDateChange"
          class="date-picker"
        />
      </div>
    </div>
    <div v-if="fromDate || toDate" class="selected-range">
      <p class="selected-value">
        From: {{ formatDate(fromDate) || 'Not selected' }}
      </p>
      <p class="selected-value">
        To: {{ formatDate(toDate) || 'Not selected' }}
      </p>
    </div>
  </div>
</template>

<style scoped>
.picker-container {
  background: #fff;
  padding: 1.5rem;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  min-width: 250px;
  text-align: center;
}

.date-range {
  min-width: 300px;
}

h2 {
  margin: 0 0 1rem 0;
  font-size: 1.5rem;
}

.date-picker {
  font-size: 1.2rem;
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 4px;
  cursor: pointer;
  width: 200px;
  margin-bottom: 1rem;
}

.date-picker:focus {
  outline: 2px solid #646cff;
  border-color: #646cff;
}

.range-inputs {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.date-input {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
}

.date-input label {
  font-weight: 500;
}

.selected-value {
  font-size: 1.2rem;
  color: #666;
  margin: 0;
}

.selected-range {
  margin-top: 1rem;
  padding-top: 1rem;
  border-top: 1px solid #eee;
}

@media (prefers-color-scheme: dark) {
  .picker-container {
    background: #2a2a2a;
  }
  
  .date-picker {
    background: #1a1a1a;
    color: #fff;
    border-color: #444;
  }
  
  .selected-value {
    color: #999;
  }

  .selected-range {
    border-top-color: #444;
  }
}
</style>