<template>
  <div id="app">
    <HelloWorld msg="欢迎来到DNF模拟器,开始之前将随机赋予0到10点属性值.点击方框触发普通事件,点击下方按钮后再点击方框可触发特殊事件,属性可通过事件获得,同时影响特殊事件,点击此处开始"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
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
