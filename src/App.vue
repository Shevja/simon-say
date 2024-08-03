<script setup>
import { ref } from 'vue';
import TheBoard from './components/TheBoard.vue';

const tilesCount = ref(4)
const dificult = ref(0)



</script>

<template>
  <div class="container">
    <TheBoard :tilesCount="tilesCount" :dificult="dificult" />
  </div>
</template>

<style scoped>

</style>
