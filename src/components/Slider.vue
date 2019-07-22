<template>
    <div class="slider">
        <blockquote class="slider__quote">
          <vue-typer text="There’s no such thing as good weather, or bad weather.
There’s just weather and your attitude towards it..." :repeat="0">
          </vue-typer>
        </blockquote>
    </div>
</template>

<script>
import { VueTyper } from 'vue-typer';

export default {
  components: { VueTyper },
}
</script>

<style lang="scss">
  .slider {
    position: relative;
    background: url('../assets/img/sky-texture-bg.jpg') center repeat-x;
    background-size: 120%;
    width: 100%;
    height: 350px;
    color: var(--color-white);

    &__quote {
      position: absolute;
      width: 60%;
      left: 50%;
      top: 40%;
      transform: translate(-50%, -50%);
      text-align: center;
    }

    .vue-typer {
      color: var(--color-white);
    }
    .custom.char {
      background-color: var(--color-white);
    }
  }
</style>
