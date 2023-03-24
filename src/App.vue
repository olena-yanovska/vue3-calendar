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
} from 'date-fns';

export default {
  data() {
    return {
      choosenDate: new Date(),
    }
  },
  computed: {
    currentDay() {
      return format(this.choosenDate, 'dd');
    },

    choosenMonth() {
      return format(this.choosenDate, 'MMMM yyyy');
    },
    daysOfWeek() {
      return ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];
    },
    days() {
      const startOfMonthDate = startOfMonth(this.choosenDate);
      const endOfMonthDate = endOfMonth(this.choosenDate);
      const startOfWeekDate = startOfWeek(startOfMonthDate);
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
  }
}
</script>

<template>
  <div class="calendar">
    <div class="header">
      <button class="button-left" @click="prevMonth"></button>
      <span>{{ choosenMonth }}</span>
      <button class="button-right" @click="nextMonth"></button>
    </div>

    <div class="weekdays">
      <span v-for="day in daysOfWeek">{{ day }}</span>
    </div>

    <div class="days">
      <span
        v-for="(day, index) in days"
        :key="index"
        @click="select(day)"
      >
        {{ day.getDate() }}
      </span>
    </div>
  </div>
</template>

<style>
.calendar {
  display: flex;
  flex-direction: column;
  width: 250px;
  border: 1px solid black;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px;
}

.weekdays {
  display: flex;
  justify-content: space-between;
  padding: 8px;
}

.days {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 2px;
  padding: 8px;
}

.today {
  background-color:aquamarine;
}

.selected {
  background-color: #008000;
  color: #fff;
}

.button-left {
  background-image: url(./assets/images/arrow-left.svg);
  width: 30px;
  height: 30px;

  border-radius: 50px;

  background-position: center;
  background-repeat: no-repeat;
}

.button-right {
  background-image: url(./assets/images/arrow-right.svg);

  width: 30px;
  height: 30px;

  border-radius: 50px;

  background-position: center;
  background-repeat: no-repeat;
}
</style>
