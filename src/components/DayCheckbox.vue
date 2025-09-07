<template>
  <div class="d-flex flex-column align-center">
    <span class="day">{{ day }}</span>
    <div class="checkbox" />
  </div>
</template>

<script setup lang="ts">
  export type DayCheckbox = string | null

  const props = defineProps({
    day: {
      type: String as PropType<DayCheckbox>,
      required: true,
    },
  })
</script>

<style lang="scss" scoped>
.day{
  line-height: normal;
  font-size: 13px;
  margin-bottom: 5px;
  //letter-spacing: 1px;
}
.checkbox{
  width: 14px;
  height: 14px;
  border: 2px solid #000000;
  border-radius: 3px;
}
</style>
