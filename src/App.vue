<template>
  <div>
    <Header />

    <ImageManipulation class="" />
  </div>
</template>

<script>
import Header from './components/Header.vue';
import ImageManipulation from './components/ImageManipulation.vue';

export default {
  name: 'App',
  components: {
    Header,
    ImageManipulation,
  },
};
</script>

<style>
</style>
