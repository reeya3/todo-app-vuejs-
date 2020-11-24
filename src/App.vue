<template>
  <div id="app" class="container">
    <img src="./assets/logo.png" class="logo">
    <HelloWorld/>
    <TodoList/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import TodoList from './components/TodoList'

export default {
  name: 'App',
  components: {
    HelloWorld,
    TodoList
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  font-size: 19px;
  margin-top: 60px;
}
* {
  box-sizing: border-box;
}
.logo {
  display: block;
  margin: 20px auto;
  height: 75px;
}
.container {
  max-width: 600px;
  margin: 0px auto;
}

</style>
