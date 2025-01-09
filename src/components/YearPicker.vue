<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps<{
  modelValue: string
}>()

const emit = defineEmits<{
  'update:modelValue': [value: string]
}>()

const handleYearChange = (event: Event) => {
  const target = event.target as HTMLSelectElement
  emit('update:modelValue', target.value)
}

const currentYear = new Date().getFullYear()
const yearOptions = computed(() => 
  [currentYear - 2, currentYear - 1, currentYear]
)
</script>

<template>
  <div class="picker-container">
    <select 
      :value="modelValue"
      @change="handleYearChange"
      class="year-picker"
    >
      <option v-for="year in yearOptions" :key="year" :value="year">
        {{ year }}
      </option>
    </select>
  </div>
</template>

<style scoped>
.picker-container {
  background: #fff;
  padding: 1rem;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  min-width: 200px;
}

.year-picker {
  font-size: 1.2rem;
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 4px;
  cursor: pointer;
  width: 100%;
}

.year-picker:focus {
  outline: 2px solid #646cff;
  border-color: #646cff;
}

@media (prefers-color-scheme: dark) {
  .picker-container {
    background: #2a2a2a;
  }
  
  .year-picker {
    background: #1a1a1a;
    color: #fff;
    border-color: #444;
  }
}
</style>