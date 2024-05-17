<!-- App.vue -->
<template>
  <div id="app">
    <h1 class="app-title">Welcome to My Awesome Slider</h1>
    <SliderOne />
    <SlideTwo />
  </div>
</template>

<script>
import SliderOne from './components/SliderOne.vue';
import SlideTwo from './components/SlideTwo.vue';

export default {
  name: 'App',
  components: {
    SliderOne,
    SlideTwo
  }
};
</script>

<style>
#app {
  font-family: Arial, Helvetica, sans-serif;
  text-align: center;
}
.app-title {
  color: #fff;
  font-size: 32px;
  margin-bottom: 30px;
  padding: 20px;
  background: #000;
}
</style>
