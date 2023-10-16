<template>
  <thead class="bg-[#ccc]">
    <tr>
      <th class="cursor-pointer select-none" @click="prev">&laquo;</th>
      <!-- <th colspan="5">{{ year }}/{{ stringMonth }}</th> -->
      <th colspan="5">
        <select name="year" class="w-1/4 text-right bg-[#ccc] appearance-none">
          <option selected>{{ year }}</option>
        </select>
        <span class="inline-block px-1">/</span>
        <select
          name="month"
          class="w-1/5 text-left bg-[#ccc] appearance-none"
          :value="modelValue"
          @input="$emit('update:modelValue', $event.target.value)"
        >
          <option
            v-for="month in months"
            :key="month"
            :selected="month.isThisMonth"
          >
            {{ month.mon }}
          </option>
        </select>
      </th>
      <th class="cursor-pointer select-none" @click="next">&raquo;</th>
    </tr>
    <tr>
      <th>Sun</th>
      <th>Mon</th>
      <th>Tue</th>
      <th>Wed</th>
      <th>Thr</th>
      <th>Fri</th>
      <th>Sat</th>
    </tr>
  </thead>
</template>

<style>
/* aaa */
</style>

<script setup lang="ts">
const props = defineProps(["year", "stringMonth", "months", "modelValue"]);
const emits = defineEmits(["prev", "next", "update:modelValue"]);
const prev = () => {
  emits("prev");
};
const next = () => {
  emits("next");
};
</script>
