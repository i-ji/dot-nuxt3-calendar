<template>
  <div class="mt-10">
    <table class="border-2 border-[#ccc] mx-auto w-[292px]">
      <Thead
        :year="year"
        :stringMonth="stringMonth"
        :months="months"
        @prev="prev"
        @next="next"
      ></Thead>
      <Tbody :weeks="weeks"></Tbody>
      <Tfoot @click="backToday"></Tfoot>
    </table>
  </div>
</template>

<style>
.disabledDate {
  opacity: 0.3;
}

.today {
  font-weight: bold;
}
</style>

<script setup lang="ts">
const today = ref<Date>(new Date());
const year = ref<number>(today.value.getFullYear());
const month = ref<number>(today.value.getMonth());

const months = ref<{ mon: number; isThisMonth: boolean }[]>([]);
for (let i = 1; i <= 12; i++) {
  months.value.push({ mon: i, isThisMonth: false });
}
months.value[month.value].isThisMonth = true;

const stringMonth = ref<string>(String(month.value + 1).padStart(2, "0"));

// 前月分のカレンダー
function getCalendarHead() {
  const dates = ref<{ date: number; isToday: boolean; isDisabled: boolean }[]>(
    []
  );

  const d = ref<number>(new Date(year.value, month.value, 0).getDate());
  const n = ref<number>(new Date(year.value, month.value, 1).getDay());

  for (let i = 0; i < n.value; i++) {
    dates.value.unshift({
      date: d.value - i,
      isToday: false,
      isDisabled: true,
    });
  }
  return dates.value;
}

// 次月分のカレンダー
function getCalendarTail() {
  const dates = ref<{ date: number; isToday: boolean; isDisabled: boolean }[]>(
    []
  );

  const lastDay = ref<number>(
    new Date(year.value, month.value + 1, 0).getDay()
  );

  for (let i = 1; i < 7 - lastDay.value; i++) {
    dates.value.push({
      date: i,
      isToday: false,
      isDisabled: true,
    });
  }

  return dates.value;
}

// 今月分のカレンダー
function getCalendarBody() {
  const dates = ref<{ date: number; isToday: boolean; isDisabled: boolean }[]>(
    []
  );

  const lastDate = ref<number>(
    new Date(year.value, month.value + 1, 0).getDate()
  );

  for (let i = 1; i <= lastDate.value; i++) {
    dates.value.push({ date: i, isToday: false, isDisabled: false });
  }

  // console.log(dates.value[today.value.getDate() - 1]);
  if (
    year.value === today.value.getFullYear() &&
    month.value === today.value.getMonth()
  ) {
    dates.value[today.value.getDate() - 1].isToday = true;
  }

  return dates.value;
}

// カレンダーの描画処理
const createCalendar = () => {
  const dates = ref<object[]>([
    ...getCalendarHead(),
    ...getCalendarBody(),
    ...getCalendarTail(),
  ]);
  const weeks = ref<object[]>([]);
  const weekCount = dates.value.length / 7;

  for (let i = 0; i < weekCount; i++) {
    weeks.value.push(dates.value.splice(0, 7));
  }

  return weeks.value;
};

const weeks = ref<object[]>(createCalendar());

// prev処理
const prev = () => {
  month.value--;
  if (month.value < 0) {
    year.value--;
    month.value = 11;
  }

  weeks.value = createCalendar();
};

// next処理
const next = () => {
  month.value++;
  if (month.value > 11) {
    year.value++;
    month.value = 0;
  }

  weeks.value = createCalendar();
};

// todayに戻る処理
const backToday = () => {
  year.value = today.value.getFullYear();
  month.value = today.value.getMonth();
  weeks.value = createCalendar();
};
</script>
