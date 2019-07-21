<template>
  <div>
    <slider />
    <div class="container">
      <location-input @onSearch="onSearch" @search="setLocation" :error="errorMessage"/>
      <transition name="day" appear>
        <weather-survey
          :days="days"
          :key="days.length"
          v-if="days.length > 0"
          @skyFilter="(e) => filters.sky = e"
          @temperatureFilter="(e) => filters.temperature = e"
          @windFilter="(e) => filters.windSpeed = e"/>
      </transition>
      <transition name="day" appear>
        <weather-cards :days="filteredDays" :key="filteredDays.length"/>
      </transition>
    </div>

  </div>
</template>

<script>
import { filter } from 'lodash';
import axios from 'axios';
import Slider from './Slider';
import LocationInput from './LocationInput';
import WeatherCards from './WeatherCards';
import WeatherSurvey from './WeatherSurvey';

export default {
  components: {
    Slider,
    LocationInput,
    WeatherSurvey,
    WeatherCards,
  },

  data() {
    return {
      days: [],
      errorMessage: '',
      location: '',
      filteredDays: [],
      filters: {
        sky: null,
        temperature: null,
        wind: null,
      },
    };
  },

  watch: {
    filters: {
      handler: function () {
        let updatedDays = this.$data.days;

        const sky = this.$data.filters.sky;
        const temperature = this.$data.filters.temperature;
        const wind = this.$data.filters.wind;

        if (sky !== null && sky !== '') {
          updatedDays = filter(updatedDays, function(day) {
            return day.sky === sky;
          }.bind(this));
        }

        if (temperature !== null) {
          updatedDays = filter(updatedDays, function(day) {
            return day.temperature === temperature;
          }.bind(this));
        }

        if (wind !== null) {
          updatedDays = filter(updatedDays, function(day) {
            return day.wind === wind;
          }.bind(this));
        }

        this.$data.filteredDays = updatedDays;
      },
      deep: true,
    },

    resetFilters: {
      handler: function () {
        this.$data.filteredDays = this.$data.days;
      },
      deep: true,
    },
  },

  methods: {
    onSearch(value) {
      this.$data.location = value;
    },
    getFormattedUrl(city, code) {
      return `https://api.openweathermap.org/data/2.5/forecast?q=${city},${code}&mode=json&units=metric&APPID=06ad7ba97e527890c573778af7fe8b94`
    },
    setLocation() {
      const splited = this.$data.location.split(',');
      const city = splited[0] && splited[0].trim();
      const countryCode = splited[1] && splited[1].trim();

      axios
        .get(this.getFormattedUrl(city, countryCode))
        .then((response) => {
          this.$data.days = response.data.list
            .filter(day => day.dt_txt.includes('15:00'))
            .map(day => ({
              date: day.dt_txt.split(' ')[0],
              temperature: Math.round(day.main.temp),
              sky: day.weather[0].main,
              description: day.weather[0].description,
              windSpeed: Math.round(day.wind.speed),
              location: city.toUpperCase(),
            }));
          this.$data.error = '';
          this.$data.filteredDays = this.$data.days;
        })

        .catch(() => {
          this.$data.errorMessage = 'Please, try valid location';
        })
        .finally(() => {
          this.$forceUpdate();
        });
    },
  },

};
</script>

<style>
  .container {
    width: 1199px;
    margin: 0 auto;
  }
</style>
