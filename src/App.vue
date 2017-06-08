<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'app'
}
</script>

<style>
html,body{
  margin: 0;
  padding: 0;
}
#app {
  font-size: 14px;
  font-family: Helvetica;
  width: 100%;
  height: 100%;
}
</style>
