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
        {{ month }}
        <span v-if="year != todayYear">
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
              :class="{'is-weekend': (firstDayOfMonth + day + 1) % 7 < 2}"
          >
            {{ day }}
          </div>
          <Events
              :events="events"
              :day="day"
              :date-context="dateContext"
          />
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
import Events from "@/components/Events";
export default {
  name: "Calendar",
  components: { Events },
  props: {
    events: {
      type: Array,
      default: () => []
    }
  },
  data () {
    return {
      dateContext: moment(),
      days_of_week: ['Понедельник', 'Вторник', 'Среда', 'Четверг', 'Пятница', 'Суббота', 'Воскресенье'],
      months: ['Январь', 'Февраль', 'Март', 'Апрель', 'Май', 'Июнь', 'Июль', 'Август', 'Сентябрь', 'Октябрь', 'Ноябрь', 'Декабрь']
    }
  },
  methods: {
    nextMonth () {
      this.dateContext = moment(this.dateContext).add(1, 'month');
    },
    previousMonth () {
      this.dateContext = moment(this.dateContext).subtract(1, 'month');
    },
    classDay (date) {
      if (this.year === this.todayYear && this.month === this.todayMonth && date === this.todayDay) {
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
      return this.dateContext.format('MMMM')
    },
    daysInMonth () {
      return this.dateContext.daysInMonth()
    },
    daysInPreviousMonth () {
      return moment(this.dateContext).subtract(1, 'month').daysInMonth()
    },
    firstDayOfMonth () {
      let a = moment({day: 1}) //.weekday()
      return a.weekday()
    },
    daysInNextMonth () {
      return 7 - (this.firstDayOfMonth + this.daysInMonth) % 7
    },
    todayDay () {
      return moment().get('date')
    },
    todayMonth () {
      return moment().format('MMMM')
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
  display: flex;
  flex-direction: column;
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

.today span {
  color: #42b983;
}
.month-days .day.today {
  border-color: #2b2c2b;
}


</style>
