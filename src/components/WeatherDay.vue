<template>
  <transition name="day">
    <li :style="{ background: skyStateBg }"
        class="day">
      <h3 class="day__weather__location">{{ location }}</h3>
      <h2 class="day__date">{{ date }}</h2>
      <ul class="day__weather">
        <li class="day__weather__temperature">{{ temp }}&#176;C</li>
        <li class="day__weather__description">{{ description }}</li>
        <li class="day__weather__sky">Wind: <span>{{ wind }} km/h</span></li>
      </ul>
    </li>
  </transition>
</template>

<script>
export default {
  props: {
    location: '',
    sky: '',
    date: '',
    description: '',
    temp: 0,
    wind: 0,
  },
  data() {
    return {
      bgRainy: 'linear-gradient(#4065A4, #D0AEA7)',
      bgClouds: 'linear-gradient(#5C97B5, #A4BBC9)',
      bgSnow: 'linear-gradient(#777C86, #FFFFFF)',
      bgClearDefault: 'linear-gradient(#51A4DB, #74BAE1)',
    };
  },

  computed: {
    rainySky() {
      return this.sky === 'Rain';
    },
    cloudySky() {
      return this.sky === 'Clouds';
    },
    snowSky() {
      return this.sky === 'Snow';
    },
    skyStateBg() {
      if (this.rainySky) {
        return this.bgRainy;
      }
      if (this.cloudySky) {
        return this.bgClouds;
      }
      if (this.snowSky) {
        return this.bgSnow;
      }
      return this.bgClearDefault;
    },
  },
};
</script>

<style lang="scss">
    .day {
      position: relative;
      display: flex;
      flex-direction: column;
      text-align: center;
      align-items: center;
      min-width: 220px;
      min-height: 300px;
      margin: 1rem 0 0 1rem;
      padding-top: 0.75rem;
      color: var(--color-white);
      border-radius: 6px;
      box-shadow: 0 1px 3px rgba(0, 0, 0, 0.3);
      backface-visibility: hidden;
      transform-origin: 10% 50%;
      z-index: 1;

      &__date, &__weather__temperature {
        font-size: 1.6rem;
        font-weight: 500;
        margin-bottom: 35px;
      }

      &__weather__location {
        font-size: 1.3rem;
      }

      &__weather__temperature {
        font-size: 3rem;
        line-height: 3rem;
      }

      &__weather__description {
        font-size: 1.4rem;
        margin-bottom: 2rem;
      }
      &__weather__sky {
        span {
          font-weight: 600;
        }
      }
    }
</style>
