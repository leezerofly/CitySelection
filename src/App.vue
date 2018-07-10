<template>
  <div id="app">
    <el-container>
      <el-header>
        <Search/>
      </el-header>
      <el-main>
        <PositionBox msg=""/>
        <CityList/>
      </el-main>
    </el-container>
  </div>
</template>

<script>
import PositionBox from './components/PositionBox.vue'
import Search from './components/Search.vue'
import CityList from './components/CityList.vue'

export default {
  name: 'app',
  components: {
    PositionBox,
    Search,
    CityList
  }
}
</script>

<style>
#app {
  font-family: "Helvetica Neue",Helvetica,"PingFang SC","Hiragino Sans GB","Microsoft YaHei","微软雅黑",Arial,sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-color: #EBEEF5;
  color: #2c3e50;
}
.el-header {
  background-color: white;
  color: #333;
  font-size: 14px;
  line-height: 60px;
  text-align: center;
}
</style>
