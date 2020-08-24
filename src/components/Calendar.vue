<template>
  <div class="calendar-month">
    <div class="calendar-month-header">
      <DateIndicator
        :selected-date="selectedDate"
        class="calendar-month-header-selected-month"
      />
      <DateSelector
        :current-date="today"
        :selected-date="selectedDate"
        @dateSelected="selectDate"
      />
    </div>
    <Week/>
    <ol class="days-grid">
      <Day
        v-for="day in days"
        :key="day.date"
        :day="day"
        :is-today="day.date === today"
      />
    </ol>
  </div>
</template>

<script>
import weekOfYear from 'dayjs/plugin/weekOfYear';
import DateIndicator from './DateIndicator';
import weekday from 'dayjs/plugin/weekday';
import DateSelector from './DateSelector';
import Week from './Week';
import Day from './Day';

import dayjs from 'dayjs';

dayjs.extend(weekday);
dayjs.extend(weekOfYear);

export default {
  name: 'CalendarMonth',
  components: {
    Day,
    DateIndicator,
    DateSelector,
    Week
  },
  data() {
    return {
      selectedDate: dayjs()
    };
  },
  computed: {
    days() {
      return [
        ...this.previousMonthDays,
        ...this.currentMonthDays,
        ...this.nextMonthDays
      ];
    },
    today() {
      return dayjs().format('YYYY-MM-DD');
    },
    month() {
      return Number(this.selectedDate.format('M'));
    },
    year() {
      return Number(this.selectedDate.format('YYYY'));
    },
    numberOfDaysInMonth() {
      return dayjs(this.selectedDate).daysInMonth();
    },
    currentMonthDays() {
      return [...Array(this.numberOfDaysInMonth)].map((day, index) => {
        return {
          date: dayjs(`${this.year}-${this.month}-${index + 1}`).format(
            'YYYY-MM-DD'
          ),
          isCurrentMonth: true
        };
      });
    },
    previousMonthDays() {
      const firstDayOfTheMonthWeekday = this.getWeekday(
        this.currentMonthDays[0].date
      );
      const previousMonth = dayjs(`${this.year}-${this.month}-01`).subtract(
        1,
        'month'
      );
      const previousMonthLastMondayDayOfMonth = dayjs(
        this.currentMonthDays[0].date
      )
        .subtract(firstDayOfTheMonthWeekday - 1, 'day')
        .date();
      // Cover the first day of the month
      const visibleNumberOfDaysFromPreviousMonth = firstDayOfTheMonthWeekday
        ? firstDayOfTheMonthWeekday - 1
        : 6;

      return [...Array(visibleNumberOfDaysFromPreviousMonth)].map(
        (day, index) => {
          return {
            date: dayjs(
              `${previousMonth.year()}-${previousMonth.month() +
                1}-${previousMonthLastMondayDayOfMonth + index}`
            ).format('YYYY-MM-DD'),
            isCurrentMonth: false
          };
        }
      );
    },
    nextMonthDays() {
      const lastDayOfTheMonthWeekday = this.getWeekday(
        `${this.year}-${this.month}-${this.currentMonthDays.length}`
      );
      const nextMonth = dayjs(`${this.year}-${this.month}-01`).add(1, 'month');
      const visibleNumberOfDaysFromNextMonth = lastDayOfTheMonthWeekday
        ? 7 - lastDayOfTheMonthWeekday
        : lastDayOfTheMonthWeekday;

      return [...Array(visibleNumberOfDaysFromNextMonth)].map((day, index) => {
        return {
          date: dayjs(
            `${nextMonth.year()}-${nextMonth.month() + 1}-${index + 1}`
          ).format('YYYY-MM-DD'),
          isCurrentMonth: false
        };
      });
    }
  },
  methods: {
    getWeekday(date) {
      return dayjs(date).weekday();
    },
    selectDate(newSelectedDate) {
      this.selectedDate = newSelectedDate;
    }
  },
};
</script>

<style scoped>
.calendar-month {
  position: relative;
  background-color: #999;
  border: solid .1rem #2c3e50;
}

.day-of-week {
  color: #333;
  font-size: 1.8rem;
  background-color: #fff;
  padding-bottom: .5rem;
  padding-top: 1rem;
}

.day-of-week,
.days-grid {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
}

.day-of-week > * {
  text-align: right;
  padding-right: .5rem;
}

.days-grid {
  height: 100%;
  position: relative;
  grid-column-gap: .1rem;
  grid-row-gap: .1rem;
  border-top: solid .1rem #2c3e50;
}
</style>
