<template>
  <tbody>
    <tr v-for="week in weeks" :key="week">
      <td
        v-for="date in week"
        :key="date"
        class="first:text-red-500 last:text-blue-500"
        :class="{ disabledDate: date.isDisabled, today: date.isToday }"
      >
        {{ date.date }}
      </td>
    </tr>
  </tbody>
</template>

<script setup lang="ts">
const props = defineProps(["weeks"]);
</script>
