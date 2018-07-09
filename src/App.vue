<template>
  <div id="app">
    <el-container>
      <el-header>
        <Search/>
      </el-header>
      <el-main>
        <img src="./assets/logo.png">
        <HelloWorld msg="Welcome to Your Vue.js App"/>
        <el-button>默认按钮</el-button>
      </el-main>
    </el-container>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import Search from './components/Search.vue'

export default {
  name: 'app',
  components: {
    HelloWorld,
    Search
  }
}
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
