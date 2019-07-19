<template>
  <button class="btn-primary" @click="click">
    <slot />
  </button>
</template>

<script>
export default {
  methods: {
    click() {
      this.$emit('click');
    },
  },
};
</script>

<style lang="scss">
  .btn-primary {
    position: relative;
    width: 100%;
    height: 4rem;
    color: var(--color-white);
    font-size: 1.5rem;
    font-weight: 500;
    background-color: var(--button-color);
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.3);
    border-radius: 3px;
    text-align: center;
    transition: background-color .3s cubic-bezier(.075, .820, .165, 1);

    &:hover {
      background-color: var(--button-color--hover);
    }

    &:focus,
    &:active {
      background-color: var(--button-color--active)
    }
  }
</style>
