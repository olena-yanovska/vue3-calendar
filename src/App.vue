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
  parseISO,
} from 'date-fns';

import { zonedTimeToUtc, utcToZonedTime } from 'date-fns-tz';

export default {
  data() {
    return {
      selectedDate: null,
      choosenDate: new Date(),
      currentDate: new Date(),
      currentTime: '',
      timezone: 'Europe/London',
      events: [
        {
          title: 'Family dinner at the restaurant',
          startDate: '2023-03-26T14:00:00.000+03:00',
          endDate: '2023-03-26T15:00:00.000+03:00',
        }
      ]
    }
  },
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

    currentDayOfWeek() {
      return format(this.currentDate, 'EEEE');
    },

    days() {
      const startOfMonthDate = startOfMonth(this.choosenDate);
      const endOfMonthDate = endOfMonth(this.choosenDate);
      const startOfWeekDate = startOfWeek(startOfMonthDate, { weekStartsOn: 1 });
      const endOfWeekDate = endOfWeek(endOfMonthDate, { weekStartsOn: 1 });

      const days = [];
      let day = startOfWeekDate;

      while (day <= endOfWeekDate) {
        days.push(day);
        day = addDays(day, 1);
      }

      return days;
    }
  },
  mounted() {
    this.updateTime();

    setInterval(this.updateTime, 1000);
  },  
  methods: {
    formatDateStart(dateString) {
      const date = new Date(dateString);
      const timeInKyiv = zonedTimeToUtc(date, 'Europe/Kyiv');
      const londonTimezone = 'Europe/London';
      const timeInLondon = utcToZonedTime(timeInKyiv, londonTimezone);
      return format(timeInLondon, 'dd.MM.yyyy HH:mm');
    },
    formatDateEnd(dateString) {
      const date = new Date(dateString);
      const londonTimezone = 'Europe/London';
      const londonTime = utcToZonedTime(date, londonTimezone);
      return format(londonTime, 'HH:mm');
    },
    updateTime() {
      const now = new Date();
      const timeInKyiv = zonedTimeToUtc(now, 'Europe/Kyiv');
      const londonTimezone = 'Europe/London';
      const timeInLondon = utcToZonedTime(timeInKyiv, londonTimezone);
      const formattedTime = format(timeInLondon, 'h:mm:ss a');
      this.currentTime = formattedTime;
    },

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
    },

    hasEvent(day) {
      const formattedDay = format(day, 'yyyy-MM-dd');

      let event = this.events.find(e => {
        const formattedStartDay = format(parseISO(e.startDate), 'yyyy-MM-dd');
        return formattedStartDay === formattedDay;
      });

      if (event) {
      return event;
      } else {
      return false;
      }
    }
  }
}
</script>

<template>
  <div class="page">
    <div class="content">
      <p class="is-size-4 has-text-centered">
        Today is 
        {{ currentDayOfWeek }},
        {{ currentDayWithEnding }} of
        {{ currentMonth }}
        {{ currentYear }}
      </p>

      <p class="is-size-6 has-text-centered">
        Time: 
        {{ currentTime }} 
        {{ timezone }}
      </p>
    </div>

    <div class="calendar">
      <div class="has-text-light has-background-success box">
        <div class="header">
          <button class="button is-rounded button-left" @click="prevMonth">
        </button>

        <span class="is-size-4">{{ choosenMonth }}</span>

        <button class="button is-rounded button-right" @click="nextMonth">
        </button>
        </div>
      </div>

      <div class="box">
        <div class="days">
          <span class="has-text-weight-semibold has-text-centered" v-for="day in daysOfWeek">
            {{ day }}
          </span>
        </div>

        <div class="days-container">
          <div class="days">
            <span
              v-for="(day, index) in days"
              :key="day"
              class="day has-text-centered is-size-6 is-clickable"
              :class="{ 
                'has-text-light has-background-success': isToday(day), 
                'has-background-grey-lighter': isSelected(day),
                'has-text-dark has-background-warning': this.hasEvent(day),
              }"

              @click="select(day)"
            > 
            <div>
              {{ day.getDate() }}
              <div v-if="this.hasEvent(day)" class="event box has-text-left">
                <p class="has-text-info  is-size-5 has-text-weight-medium">
                  Events:
                </p>
                <p class="has-text-weight-medium is-size-6">{{ this.hasEvent(day).title }}</p>
                <p class="is-size-7">
                  {{ formatDateStart(this.hasEvent(day).startDate) }}
                   - 
                   {{ formatDateEnd(this.hasEvent(day).endDate) }}
                   (Europe/London)
                </p>
              </div>
            </div>

          </span>
        </div>
      </div>
      </div>
    </div>
  </div>
</template>

<style>
.event {
  position: absolute;
  bottom: -140px;
  left: 0;
  width: 350px;
  min-height: 100px;
}

.page {
  display: flex;
  flex-direction: column;
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
  position: relative;
  flex-direction: column;
  width: 350px;
}

.header {
  display: flex;
  flex-direction: row;

  justify-content: space-between;
  align-items: center;
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
