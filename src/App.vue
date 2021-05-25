<template>
  <div id="app">
    <SendEmail />
  </div>
</template>

<script>
import SendEmail from "./components/SendEmail";

export default {
  name: 'App',
  components: {
      SendEmail,
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
