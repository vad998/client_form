<template>
  <div id="app">
    <NewClient />
  </div>
</template>

<script>
import NewClient from './components/NewClient.vue'

export default {
  name: 'App',
  components: {
    NewClient
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;500&display=swap');
html {
  background-color: #FAFAFA;
  font-family: Roboto;
  font-weight: 400;
  font-style: normal;
}
body {
  margin: 0;
}
</style>
