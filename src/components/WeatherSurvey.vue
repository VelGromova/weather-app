<template>
  <div>
    <nav class="nav">
      <ul class="nav__controls">
        <li
          v-for="(filterItem, index) in filters"
          :key="'menu-' + index"
          class="nav__label"
          :class="{ active: filterItem.isActive }"
          @click="changeFilterStatus(index, !filterItem.isActive)"
        >
          {{ filterItem.title }}
        </li>
        <li class="nav__label nav__label--clear" @click="clearFilters">Clear filters</li>
      </ul>
    </nav>

    <menu v-for="(filterItem, index) in filters" class="filters" v-if="filterItem.isActive" :key="'showDropDown-' + index">
      <ul v-if="index === 'sky'">
        <li
          v-for="(weather, skyIndex) in skyStates"
          v-if="index === 'sky'"
          class="filters__item"
          :key="'skyIndex-' + skyIndex"
          :class="{ 'filters__item--active': filterItem.isActive }"
          @click="filters.sky.value = weather"
        >
          {{ weather }}
        </li>
      </ul>
      <ul v-if="index === 'temperature'">
        <li>
          <output>
            <label>{{ index }}:&nbsp; {{ filterItem.value }}</label>
          </output>
          <input v-model="filterItem.value" :max="maxTempValue" :min="minTempValue" class="filters__range" type="range"/>
        </li>
      </ul>
      <ul v-if="index === 'wind'">
        <li>
          <output>
            <label>{{ index }}:&nbsp; {{ filterItem.value }}</label>
          </output>
          <input v-model="filterItem.value" :max="maxWindValue" :min="minWindValue" class="filters__range" type="range"/>
        </li>
      </ul>
    </menu>
  </div>
</template>

<script>

export default {
  props: {
    days: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      filters: {
        sky: { title: 'Sky', isActive: false, value: '' },
        wind: { title: 'Wind', isActive: false, value: 0 },
        temperature: { title: 'Temperature', isActive: false, value: 0 },
      },
    };
  },
  watch: {
    filters: {
      handler() {
        const sky = this.$data.filters.sky.value;
        const temperature = parseInt(this.$data.filters.temperature.value);
        const wind = parseInt(this.$data.filters.wind.value);

        this.$emit('filterHandler', sky, temperature, wind);
      },
      deep: true,
    },
  },

  methods: {
    clearFilters() {
      this.$data.filters.sky.value = '';
      this.$data.filters.wind.value = 0;
      this.$data.filters.temperature.value = 0;
    },
    changeFilterStatus(index, status) {
      for (const filter in this.$data.filters) {
        this.$data.filters[filter].isActive = false;
      }

      this.$data.filters[index].isActive = status;
    },

  },
  computed: {
    skyStates() {
      const allSkyValues = this.days;
      return [...new Set(allSkyValues.map(day => day.sky))];
    },

    // temperature
    getTempValue() {
      return this.days.map(day => day.temperature);
    },
    maxTempValue() {
      return this.filters.temperature.value = Math.max(...this.getTempValue);
    },
    minTempValue() {
      return this.filters.temperature.value = Math.min(...this.getTempValue);
    },

    // wind
    getWindValue() {
      return this.days.map(day => day.windSpeed);
    },
    maxWindValue() {
      return this.filters.wind.value = Math.max(...this.getWindValue);
    },
    minWindValue() {
      return this.filters.wind.value = Math.min(...this.getWindValue);
    },
  },

};
</script>

<style lang="scss">
  .nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
    white-space: nowrap;
    margin: 0 1rem;
    padding: 2rem 0.5rem 1rem;
    border-bottom: 1px solid #c5d0d1;

    &__controls { display: flex }

    &__label {
      position: relative;
      margin-left: 1rem;
      text-transform: capitalize;
      z-index: 1;
      padding: 3px 10px;
      cursor: pointer;

      &:hover {
        background-color: rgba(164, 187, 201, 0.1);
        border-radius: 6px;
      }

      &:after {
        content: '\00d7';
        display: inline-block;
        color: transparent;
        width: 0.5rem;
        font-weight: 400;
        transform: scale(0);
        transition: transform 150ms ease-in-out;
      }

      &--clear {
        color: #f68185;
        margin-left: 3rem;
        transform: translate3d(-25%, 0, 0);
        transition: all 275ms ease-in-out;
      }

      &--filter ~ &--clear {
        opacity: 1;
        transform: translate3d(0, 0, 0);
      }

      &--filter::after {
        transform: scale(1);
        content: '\2022';
        color: #46d2c4;
      }
    }
    .active {
      font-weight: 600;
      &:after {
        content: '\00d7';
        color: #f68185;
        transform: scale(1)
      }
    }
  }

  .filters {
    padding: 0 1rem;
    display: flex;
    flex-wrap: wrap;
    align-items: flex-start;

    &__item {
      margin-top: 0.5rem;
      margin-right: 0.5rem;
      padding: 0.25rem 0.5rem;
      border: 1px solid #c5d0d1;
      border-radius: 6px;
      font-size: 0.8rem;
      line-height: 1.35;
      cursor: pointer;
      transition: all 275ms;

      &:hover { border-color: #379a93 }

      &--active {
        color: white;
        border-color: #379a93;
        background-color: #379a93;
      }
    }

    &__range {
      width: 200px;
      margin-top: 1rem;
      color: inherit;

      &::-webkit-slider-thumb {
        width: 0.8rem;
        height: 0.8rem;
        margin-top: calc(-0.4rem + 2px);
        border-radius: 100%;
        background-color: #3d3e40;
      }

      &::-webkit-slider-runnable-track {
        width: 100%;
        height: 4px;
        background-image: linear-gradient(to right, #fff, #46d2c4);
      }
    }
    ul {
      display: flex;
    }
  }

</style>
