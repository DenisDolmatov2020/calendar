<template>
  <div>

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

  }

}
</script>

<style scoped>

div:focus {
  color: wheat;
  overflow-x: visible;
}
</style>
