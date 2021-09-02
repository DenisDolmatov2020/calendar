<template>
  <div class="calendar">
    <div class="calendar-header">
      <button
        class="calendar-header-btn"
        @click="previousMonth"
      >
          (
      </button>
      <p>
        {{ months[month] }}
        <span v-if="year !== todayYear">
           {{ year }}
        </span>
      </p>
      <button
        class="calendar-header-btn"
        @click="nextMonth"
      >
        )
      </button>
    </div>
    <div class="calendar-main">
      <div class="week-days">
        <div
            v-for="day in days_of_week"
            :key="`day_of_month_${day}`"
            class="day"
        >
          {{ day }}
        </div>
      </div>

      <div class="month-days">
        <div
            v-for="previous_day in firstDayOfMonth - 1"
            :key="`previous_day_${previous_day}`"
            class="day previous_day"
        >
          <div
            class="number"
            :class="{'is-weekend': previous_day % 7 === 6}"
          >
            {{ daysInPreviousMonth - firstDayOfMonth + previous_day + 1 }}
          </div>
        </div>
        <div
            v-for="day in daysInMonth"
            :key="day"
            :class="['day', classDay(day)]"
        >
          <div
              class="number"
              :class="{'is-weekend': (firstDayOfMonth + day) % 7 < 2}"
          >
            {{ day }}
          </div>
          <div
              v-for="event in eventsList(day)"
              :key="event.id"
              class="event"
              :class="event.type"
          >
            <div class="event-text">
              {{ event.date.hour }}:{{ event.date.minute }} {{ event.title }}
            </div>
          </div>
        </div>
        <div
          v-for="(next_day, index) in daysInNextMonth"
          :key="`next_day_${next_day}`"
          class="day previous_day"
          :class="{'is-weekend': index + 2 >= daysInNextMonth}"
        >
          <div class="number">
            {{ next_day }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import moment from 'moment';
export default {
  name: "Calendar",
  data () {
    return {
      dateContext: moment(),
      days_of_week: ['Понедельник', 'Вторник', 'Среда', 'Четверг', 'Пятница', 'Суббота', 'Воскресенье'],
      months: ['Январь', 'Февраль', 'Март', 'Апрель', 'Май', 'Июнь', 'Июль', 'Август', 'Сентябрь', 'Октябрь', 'Ноябрь', 'Декабрь'],
      events: [
        { id: 1, title: "Поход в кино", date: { year: 2021, month: 9, day: 5, hour: 3, minute: 30 }, type: 'success' },
        { id: 2, title: "Поход в кино sgergerwger", date: { year: 2021, month: 9, day: 6, hour: 18, minute: 30 }, type: 'error' },
        { id: 6, title: "Сессия парная", date: { year: 2021, month: 9, day: 6, hour: 18, minute: 30 }, type: 'warning' },
        { id: 3, title: "Поход в кино", date: { year: 2021, month: 9, day: 15, hour: 5, minute: 0 }, type: 'warning' },
        { id: 4, title: "Поход в кино", date: { year: 2021, month: 9, day: 12, hour: 8, minute: 15 }, type: 'error' },
        { id: 5, title: "Поход в кино", date: { year: 2021, month: 9, day: 8, hour: 9, minute: 30 }, type: 'success' },
      ]
    }
  },
  methods: {
    eventsList (day) {
      const events = this.events.filter(event =>
          event.date.day === day &&
          event.date.month === (this.dateContext.month() + 1) &&
          event.date.year === this.dateContext.year()
      ).sort(
          (a,b) => (a.date.hour >= b.date.hour && a.date.minute >= b.date.minute) ?
              1 : ((b.name > a.name && b.date.minute >= a.date.minute) ?
                  -1 : 0)
      )
      console.log(events)
      return events
    },
    nextMonth () {
      this.dateContext = moment(this.dateContext).add(1, 'month');
    },
    previousMonth () {
      this.dateContext = moment(this.dateContext).subtract(1, 'month');
    },
    classDay (date) {
      if (this.year === this.todayYear && this.month === moment().month() && date === moment().get('date')) {
        return 'today'
      } else {
        const date_context = moment(this.dateContext).add(date - 2, 'd')
        return date_context.format('YYYYMMDD') < moment().format('YYYYMMDD') ? 'previous_day' : ''
      }
    }
  },
  computed: {
    year () {
      return this.dateContext.format('YYYY');
    },
    month () {
      return this.dateContext.month()
    },
    daysInMonth () {
      return this.dateContext.daysInMonth()
    },
    daysInPreviousMonth () {
      return moment(this.dateContext).subtract(1, 'month').daysInMonth()
    },
    firstDayOfMonth: function () {
      const firstDay = moment(this.dateContext).subtract((this.dateContext.get('date') - 1), 'days');
      return firstDay.weekday();
    },
    daysInNextMonth () {
      return 7 - (this.firstDayOfMonth + this.daysInMonth - 1) % 7
    },
    todayYear () {
      return moment().format('YYYY');
    }
  }
}
</script>

<style scoped>

.calendar {
  background: #e3e3e3;
  margin: 0 auto;
  width: 60vw;
  border: 1px solid #333333;
  border-radius: 3em;
  padding: 20px;
}

.calendar-header {
  display: flex;
  padding: 0 1em;
  align-items: center;
  width: 100%;
}

.calendar-main {
  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;
}

.week-days {
  display: flex;
  justify-content: space-between;
  font-weight: bold;
}
.week-days .day {
  width: 14.44%;
  margin: 4px;
  text-align: center;
  cursor: pointer;
}
.week-days .day:last-child {
  margin-right: 15px;
}

.month-days {
  display: flex;
  flex-wrap: wrap;
}
.month-days .day {
  width: 120px;
  height: 120px;
  margin: 4px;
  border: 1px solid #cccccc;
  border-radius: 5px;
}
.previous_day {
  background: #333333;
  opacity: .3;
}
.month-days .day .number {
  width: 100%;
  font-weight: bold;
  text-align: end;
}
.is-weekend {
  color: mediumpurple;
}

.today div {
  color: #42b983;
}
.month-days .day.today {
  border-color: #2b2c2b;
}


.event {
  width: 100%;
  overflow-x: hidden;
  text-overflow: ellipsis;
  white-space: pre;
  position: relative;
  height: 20px;
  margin-top: 2px;
}

.event:hover {
  overflow: visible;
  width: fit-content;
  min-width: 100%;
  position: relative;

}
.success {
  background-color: green;
}

.warning {
  background-color: orange;
}

.error {
  background-color: red;
}
.event-text {
  position: absolute;
  width: 120px;
  text-overflow: ellipsis;
  overflow: hidden;
  white-space: nowrap;
}

.event-text:hover {
  overflow: visible;
  width: fit-content;
  z-index: 11
}
</style>
