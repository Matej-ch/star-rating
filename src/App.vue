<template>
  <div id="app">
    <StarRating
      :rating="4"
      :max-stars="5"
      :step="0.5"
    />

    <StarRating
      :rating="0"
      :max-stars="5"
      :step="0.5"
    />

    <StarRating
      :rating="5"
      :max-stars="5"
      :step="0.5"
    />

    <StarRating
      :rating="2.5"
      :max-stars="5"
      :step="0.5"
    />

    <StarRating
      :rating="7"
      :max-stars="10"
      :step="1"
    />
  </div>
</template>

<script setup>
import StarRating from './components/StarRating.vue'

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
