<template>
  <div id="app">
    <Calendar />
  </div>
</template>

<script>
import Calendar from "@/components/Calendar";

export default {
  name: 'App',
  components: {
    Calendar
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
