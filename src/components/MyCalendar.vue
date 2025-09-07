<template>
  <v-card class="my-calendar pa-4">
    <v-btn prepend-icon="mdi-fullscreen" @click="showFullscreen">Imprimer</v-btn>
    <section class="mb-5">
      <v-text-field
        v-model="title1"
        class="mb-3"
        label="Titre 1"
        max-width="450"
        variant="underlined"
      />
      <v-text-field
        v-model="title2"
        append-icon="mdi-plus"
        label="Titre 2"
        max-width="450"
        variant="underlined"
        @click:append="showCaption = !showCaption"
      />
      <v-text-field
        v-show="showCaption"
        v-model="caption"
        label="Détails"
        max-width="450"
        variant="underlined"
      />
    </section>

    <section>
      <p class="mb-5">
        <v-btn icon="mdi-minus" @click="addOnePrecedingMonth" />
        <v-btn icon="mdi-plus" @click="addOneFollowingMonth" />
      </p>
      <div class="months-grid">
        <MonthItem
          v-for="(month, index) in months"
          :key="index"
          :base-date="month"
        />
      </div>
    </section>
    <v-dialog v-model="dialog" fullscreen>
      <v-card class="py-8 px-9">
        <p class="calendar-h1">{{ title1 }}</p>
        <p class="calendar-h2 mb-3">{{ title2 }}</p>
        <p v-show="showCaption" class="font-weight-bold">{{ caption }}</p>

        <section class="months-grid mt-11">
          <MonthItem
            v-for="(month, index) in months"
            :key="index"
            :base-date="month"
          />
        </section>
      </v-card>
    </v-dialog>
  </v-card>
</template>

<script setup lang="ts">
  const months = ref<Date[]>([new Date()])

  function addOneFollowingMonth () {
    const lastMonth = months.value[months.value.length - 1]// eslint-disable-line unicorn/prefer-at
    const nextMonth = new Date(lastMonth.getFullYear(), lastMonth.getMonth() + 1, 1)
    months.value.push(nextMonth)
  }

  function addOnePrecedingMonth () {
    const firstMonth = months.value[0]
    const previousMonth = new Date(firstMonth.getFullYear(), firstMonth.getMonth() - 1, 1)
    months.value.unshift(previousMonth)
  }

  const title1 = ref('')
  const title2 = ref('')
  const caption = ref('')
  const showCaption = ref(false)
  const dialog = ref(false)

  function showFullscreen () {
    dialog.value = true
  }
</script>

<style lang="scss" scoped>
.my-calendar{
  aspect-ratio: 9 / 16;
  max-width:1000px
}
.months-grid{
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* 3 colonnes égales */
  justify-content: space-between;
  row-gap: 37px;
  column-gap: 44px;
}
.calendar-h1{
  font-weight:bold;
  font-size:33px;
  line-height: 41px;
}
.calendar-h2{
  font-size:17px;
  line-height: normal;
}
</style>
