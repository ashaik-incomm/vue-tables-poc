<template>
  <div data-app id="app">
    <VueTable></VueTable>
  </div>
</template>

<script>
import VueTable from './components/VueTable.vue'

export default {
  name: 'App',
  components: {
    VueTable
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
