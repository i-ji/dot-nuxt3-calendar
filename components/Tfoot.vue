<template>
  <tfoot class="bg-[#ccc] font-bold">
    <tr>
      <td colspan="7" class="cursor-pointer select-none" @click="backToday">
        Today
      </td>
    </tr>
  </tfoot>
</template>

<script setup lang="ts">
const emits = defineEmits(["backToday"]);
const backToday = () => {
  emits("backToday");
};
</script>
