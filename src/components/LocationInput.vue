<template>
  <div class="location-input">
    <input type="text"
           @keyup="getLocation"
           v-model="location"
           placeholder="Please, type your location"/>
    <span class="error-message">
      {{ error }}
    </span>
    <primary-button @click="$emit('search')">
      Submit location
    </primary-button>
  </div>

</template>

<script>
import PrimaryButton from './ui/form/PrimaryButton';

export default {
  components: { PrimaryButton },

  data() {
    return {
      location: '',
    };
  },

  props: {
    error: {
      type: String,
      default: '',
    },
  },

  methods: {
    getLocation(event) {
      this.$emit('onSearch', this.$data.location);
    },
  },
};
</script>

<style scoped lang="scss">
  .location-input {
    margin: 50px auto;
    width: 50%;
    input {
      line-height: 3;
    }
    .error-message {
      position: absolute;
      color: var(--color-red);
    }

    .btn-primary {
      margin-top: 30px;
    }
  }
</style>
