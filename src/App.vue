<template>
  <div id="app">
    <Calendar :events="events" />
  </div>
</template>

<script>
import Calendar from "@/components/Calendar";

export default {
  name: 'App',
  components: {
    Calendar
  },
  data () {
    return {
      events: [
        { id: 1, title: "Поход в кино", date: { year: 2021, month: 9, day: 5, hour: 3, minute: 30 }, type: 'success' },
        { id: 2, title: "Поход в кино sgergerwger", date: { year: 2021, month: 9, day: 6, hour: 18, minute: 30 }, type: 'error' },
        { id: 6, title: "Сессия парная", date: { year: 2021, month: 9, day: 6, hour: 18, minute: 30 }, type: 'warning' },
        { id: 3, title: "Поход в кино", date: { year: 2021, month: 9, day: 15, hour: 5, minute: 0 }, type: 'warning' },
        { id: 4, title: "Поход в кино", date: { year: 2021, month: 9, day: 12, hour: 8, minute: 15 }, type: 'error' },
        { id: 5, title: "Поход в кино", date: { year: 2021, month: 9, day: 8, hour: 9, minute: 30 }, type: 'success' },
      ]
    }
  }
}
</script>

<style>
body {
  background-color: #cccccc;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;

  margin-top: 20px;
}
</style>
