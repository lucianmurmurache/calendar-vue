<template>
  <li
    class="day"
    :class="{
      'day--not-current': !day.isCurrentMonth,
      'day--today': isToday
    }"
  >
    <span>{{ label }}</span>
  </li>
</template>

<script>
import dayjs from 'dayjs';

export default {
  name: 'CalendarMonthDayItem',

  props: {
    day: {
      type: Object,
      required: true
    },

    isCurrentMonth: {
      type: Boolean,
      default: false
    },

    isToday: {
      type: Boolean,
      default: false
    }
  },

  computed: {
    label() {
      return dayjs(this.day.date).format('D');
    }
  }
};
</script>

<style scoped>
.day {
  position: relative;
  min-height: 8rem;
  font-size: 1.6rem;
  background-color: #fff;
  padding: .5rem;
}

.day > span {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  right: 1rem;
  width: 2rem;
  height: 2rem;
}

.day--not-current {
  background-color: #2c3e50;
  color: #fff;
}

.day--today {
  padding-top: .4rem;
}

.day--today > span {
  color: #fff;
  border-radius: 2rem;
  background-color: #2c3e50;
}
</style>
