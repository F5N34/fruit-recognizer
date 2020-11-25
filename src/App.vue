<template>
  <div id="app">
  <MainUpload/>
  </div>
</template>

<script>
import MainUpload from './components/MainUpload.vue'

export default {
  name: 'app',
  components: {
    MainUpload
  }
}
</script>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Poppins&display=swap');
#app {
  font-family: 'Poppins', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

}
</style>
