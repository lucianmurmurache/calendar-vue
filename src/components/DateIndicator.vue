<template>
  <div class="date-indicator">{{ selectedMonth }}</div>
</template>

<script>
export default {
  props: {
    selectedDate: {
      type: Object,
      required: true
    }
  },

  computed: {
    selectedMonth() {
      return this.selectedDate.format('MMMM YYYY');
    }
  }
};
</script>

<style scoped>
.date-indicator {
  font-size: 2.2rem;
  font-weight: 600;
  color: #fff;
}
</style>

