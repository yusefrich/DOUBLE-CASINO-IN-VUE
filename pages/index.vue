<template>
  <!-- <roll /> -->
  <div class="d-flex align-items-center justify-content-center">
    <double />
  </div>
</template>

<script>
// import Roll from '../components/Roll.vue';
import Double from '../components/Double.vue';

export default {
  name: 'IndexPage',
  components: {
    // Roll
    Double
  }
}
</script>

<style>
* {
  margin: 0;
  box-sizing: border-box;
  padding: 0;
}

body {
  background-color: rgb(23, 18, 18);
}
</style>
