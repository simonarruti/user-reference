<template>
  <div id="app">
    <user-reference-input></user-reference-input>
  </div>
</template>

<script>
  import UserReferenceInput from './components/UserReferenceInput'

export default {
  name: 'app',
  components: { UserReferenceInput }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
