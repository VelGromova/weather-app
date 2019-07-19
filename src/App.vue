<template>
  <div id="app">
    <transition name="load" mode="out-in" appear>
      <main-layout />
    </transition>
  </div>
</template>

<script>
import '@/assets/css/app.css';
import MainLayout from './components/MainLayout';

export default {
  name: 'App',
  components: {
    MainLayout,
  },
};
</script>

<style>
html, body {
  margin: 0;
  padding: 0;
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
}
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
