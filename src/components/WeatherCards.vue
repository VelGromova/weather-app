<template>
  <ul class="days">
    <weather-day
      v-for="day in days"
      :key="day.id"
      :date="day.date"
      :sky="day.sky"
      :temp="day.temperature"
      :location="day.location"
      :wind="day.windSpeed"
      :description="day.description"
    />
  </ul>
</template>

<script>

import WeatherDay from './WeatherDay';

export default {
  components: {
    WeatherDay,
  },

  props: {
    days: {
      type: Array,
      default: () => [],
    },
  },

};
</script>

<style lang="scss">
  .days {
    display: inline-flex;
  }
</style>
