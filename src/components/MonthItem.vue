<template>
  <div class="w-fit-content">
    <p class="month-label font-weight-bold text-center">{{ label }}</p>
    <div class="days-grid">
      <template v-for="(item, idx) in days" :key="idx">
        <DayCheckbox v-if="!item.blank" :day="item.day" />
        <div v-else />
      </template>
    </div>
  </div>
</template>

<script setup lang="ts">
  import { eachDayOfInterval, endOfMonth, format, startOfMonth, startOfWeek } from 'date-fns'

  const props = defineProps({
    baseDate: {
      type: Date,
      required: true,
    },
  })

  const label = computed(() => {
    const month = props.baseDate.toLocaleString('default', { month: 'long' })
    return month.charAt(0).toUpperCase() + month.slice(1)
  })

  // generate days of the month with blanks for the first days of the week
  const days = computed(() => {
    const start = startOfMonth(props.baseDate)
    const end = endOfMonth(props.baseDate)
    const daysInMonth = eachDayOfInterval({ start, end })
    // 0 = dimanche, 1 = lundi, ..., 6 = samedi
    const blanks = (start.getDay() + 6) % 7 // pour que lundi=0, mardi=1, ..., dimanche=6
    const daysArray: { day: string | null, blank: boolean }[] = []

    for (let i = 0; i < blanks; i++) {
      daysArray.push({ day: null, blank: true })
    }
    for (const d of daysInMonth) {
      daysArray.push({ day: format(d, 'dd'), blank: false })
    }
    return daysArray
  })
</script>

<style lang="scss" scoped>
.month-label {
  font-size: 20px;
  margin-bottom: 11px;
}
.days-grid {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 11px 9px;
}
</style>
