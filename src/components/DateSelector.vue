<template>
  <div class="date-selector">
    <span @click="selectPrevious">&#8592;</span>
    <span @click="selectCurrent">Current date</span>
    <span @click="selectNext">&#8594;</span>
  </div>
</template>

<script>
import dayjs from 'dayjs';

export default {
  name: 'DateSelector',

  props: {
    currentDate: {
      type: String,
      required: true
    },

    selectedDate: {
      type: Object,
      required: true
    }
  },
  methods: {
    selectPrevious() {
      let newSelectedDate = dayjs(this.selectedDate).subtract(1, 'month');
      this.$emit('dateSelected', newSelectedDate);
    },
    selectCurrent() {
      let newSelectedDate = dayjs(this.currentDate);
      this.$emit('dateSelected', newSelectedDate);
    },
    selectNext() {
      let newSelectedDate = dayjs(this.selectedDate).add(1, 'month');
      this.$emit('dateSelected', newSelectedDate);
    }
  }
};
</script>

<style scoped>
.date-selector {
  display: flex;
  justify-content: space-between;
  width: 14rem;
  color: #fff;
  font-size: 1.6rem;
}

.date-selector > * {
  user-select: none;
  cursor: pointer;
}
</style>
