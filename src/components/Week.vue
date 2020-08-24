<template>
  <ol class="day-of-week">
    <li 
      v-for="weekday in weekdays" 
      :key="weekday"
    >
      {{ weekday }}
    </li>
  </ol>
</template>

<script>
const WEEK = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'];

export default {
  name: 'Week',
  computed: {
    weekdays() {
      return WEEK;
    }
  }
};
</script>

<style scoped>
.day-of-week {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
}

.day-of-week > * {
  text-align: right;
  padding-right: 1rem;
}
</style>
