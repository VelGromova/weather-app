<template>
  <div>
    <slider />
    <div class="container">
      <location-input @onSearch="onSearch" @search="setLocation" :error="errorMessage"/>
      <transition name="day" appear>
        <weather-survey :days="days" :key="days.length" v-if="days.length > 1"/>
      </transition>
      <transition name="day" appear>
        <weather-cards :days="days" :key="days.length"/>
      </transition>
    </div>

  </div>
</template>

<script>
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
    };
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
