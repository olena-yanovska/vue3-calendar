<script>
import { 
  format,
  addMonths, 
  addDays, 
  subMonths, 
  startOfMonth, 
  endOfMonth, 
  startOfWeek, 
  endOfWeek, 
  isSameDay,
} from 'date-fns';

export default {
  data() {
    return {
      selectedDate: null,
      choosenDate: new Date(),
      currentDate: new Date(),
      startOfWeek: 1,
    }
  },
  // mounted() {
  //   this.startOfWeek = startOfWeek(this.currentDate, { weekStartsOn: 1 });
  // },
  computed: {
    currentDay() {
      return format(this.choosenDate, 'dd');
    },

    currentDayWithEnding() {
      const day = this.currentDay;

      if (day > 20 || day < 10) {
        switch (day % 10) {
          case 1:
            return day + 'st'
          case 2:
            return day + 'nd'
          case 3:
            return day + 'rd'
        }
      }
      return day + 'th'
    },

    currentMonth() {
      return format(this.currentDate, 'MMMM yyyy');
    },

    choosenMonth() {
      return format(this.choosenDate, 'MMMM yyyy');
    },

    daysOfWeek() {
      return ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'];
    },

    days() {
      const startOfMonthDate = startOfMonth(this.choosenDate);
      const endOfMonthDate = endOfMonth(this.choosenDate);
      const startOfWeekDate = startOfWeek(startOfMonthDate, { weekStartsOn: 1 });
      const endOfWeekDate = endOfWeek(endOfMonthDate);

      const days = [];
      let day = startOfWeekDate;

      while (day <= endOfWeekDate) {
        days.push(day);
        day = addDays(day, 1);
      }

      return days;
    }
  },
  methods: {
    prevMonth() {
      this.choosenDate = subMonths(this.choosenDate, 1);
    },
    nextMonth() {
      this.choosenDate = addMonths(this.choosenDate, 1);
    },
    isToday(date) {
      return isSameDay(date, new Date());
    },
    isSelected(date) {
      return this.selectedDate && isSameDay(date, this.selectedDate);
    },
    select(date) {
      this.selectedDate = date;
    }
  }
}
</script>

<template>
  <div class="page">
    <div class="content">
      <p class="is-size-4">
        Today is 
        {{ currentDayWithEnding }} of
        {{ currentMonth }}
        {{ currentYear }}
      </p>
    </div>

    <div class="calendar">
      <div class="has-text-light has-background-success box">
        <div class="header">
          <button class="button is-rounded button-left" @click="prevMonth">
        </button>

        <div class="month is-size-4">{{ choosenMonth }}</div>

        <button class="button is-rounded button-right" @click="nextMonth">
        </button>
        </div>
      </div>

      <div class="box">
        <div class="weekdays">
          <span class="has-text-weight-semibold" v-for="day in daysOfWeek">{{ day }}</span>
        </div>

      <div class="days-container">
        <div class="days">
        <span
          v-for="(day, index) in days"
          :key="index"
          class="day has-text-centered is-size-6 is-clickable"
          :class="{ 'has-text-light has-background-success': isToday(day), ' has-background-grey-lighter': isSelected(day) }"
          @click="select(day)"
        > 
          {{ day.getDate() }}
        </span>
      </div>
      </div>
      </div>
    </div>
  </div>
</template>

<style>
.page {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100%;
}

.days-container {
height: 150px;
}

.day:hover {
  background-color: #effaf5;
}

.calendar {
  display: flex;
  flex-direction: column;
  width: 350px;
}

.header {
  display: flex;
  flex-direction: row;

  justify-content: space-between;
  align-items: center;
}

.month {
  display: inline-block;
}

.weekdays {
  display: flex;
  justify-content: space-between;
  padding: 8px;
}

.days {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
}

.button-left {
  background-image: url(./assets/images/arrow-left.svg);
  display: inline-block;

  background-position: center;
  background-repeat: no-repeat;
}

.button-right {
  background-image: url(./assets/images/arrow-right.svg);
  display: inline-block;

  background-position: center;
  background-repeat: no-repeat;
}
</style>
