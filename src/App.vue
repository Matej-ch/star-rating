<template>
  <div id="app">
    <StarRating  :rating=4 :maxstars=5 :step=0.5 update-msg="Rating Updated" />

    <StarRating  :rating=0 :maxstars=5 :step=0.5 />

    <StarRating  :rating=5 :maxstars=5 :step=0.5 />

    <StarRating  :rating=2.5 :maxstars=5 :step=0.5 />

    <StarRating  :rating=7 :maxstars=10 :step=1 update-msg="Rating destroyed"/>
  </div>
</template>

<script>
import StarRating from './components/StarRating.vue'

export default {
  name: 'App',
  components: {
    StarRating
  }
}
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
