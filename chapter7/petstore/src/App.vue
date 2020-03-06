<template>
  <div id="app">
    <!-- <img src="./assets/logo.png"> -->
    <transition name="fade" mode="out-in">
      <router-view></router-view>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
  .fade-enter-active, .fade-leave-active{
    transition: opacity .5s ease-out;
  }

  .fade-enter, .fade-leave-to{
    opacity: 0;
  }
</style>

<!--
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
-->