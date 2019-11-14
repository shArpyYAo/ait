<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <generateTemplate msg="Welcome to auto init template"/>
  </div>
</template>

<script>
import generateTemplate from './components/generateTemplate';

export default {
    name: 'app',
    components: {
        generateTemplate,
    },
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
