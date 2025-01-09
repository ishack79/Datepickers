<script setup lang="ts">
import { ref } from 'vue'
import MonthPicker from './components/MonthPicker.vue'
import YearPicker from './components/YearPicker.vue'
import RangePicker from './components/RangePicker.vue'

const currentDate = new Date()
const currentMonth = `${currentDate.getFullYear()}-${String(currentDate.getMonth() + 1).padStart(2, '0')}`

const selectedMonth = ref(currentMonth)
const selectedYear = ref(new Date().getFullYear().toString())
const dateRange = ref({
  fromDate: '',
  toDate: ''
})
</script>

<template>
  <div class="container">
    <h1>Date Pickers</h1>
    <div class="pickers-wrapper">
      <MonthPicker v-model="selectedMonth" />
      <YearPicker v-model="selectedYear" />
      <RangePicker v-model="dateRange" />
    </div>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2rem;
  padding: 2rem;
}

.pickers-wrapper {
  display: flex;
  gap: 2rem;
  flex-wrap: wrap;
  justify-content: center;
}
</style>