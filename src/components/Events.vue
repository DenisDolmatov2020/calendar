<template>
  <div>
    <div
        v-for="event in eventsList"
        :key="event.id"
        class="event"
        :class="event.type"
    >
      {{ event.title }}
    </div>
  </div>
</template>

<script>


export default {
  name: "Events",
  props: {
    events: {
      type: Array,
      default: () => []
    },
    dateContext: {
      type: Object,
      default: () => {}
    },
    day: {
      type: Number || String,
      default: null
    },
  },
  computed: {
    eventsList () {
      const events = this.events.filter(event =>
          event.date.day === this.day &&
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
  }

}
</script>

<style scoped>
.event {
  width: 100%;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: pre;
}
.success {
  background-color: greenyellow;
}

.warning {
  background-color: orange;
}

.error {
  background-color: red;
}
</style>
