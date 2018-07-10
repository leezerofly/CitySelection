<template>
  <div id="app">
    <el-container>
      <el-header>
        <Search @change-city="changeCity"/>
      </el-header>
      <el-main>
        <PositionBox :selectedCity="selectedCity" :historyCityArr="historyCityArr" @change-city="changeCity"/>
        <CityList @change-city="changeCity"/>
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
  },
  data () {
    return {
      selectedCity: {}, // 已选择的城市
      historyCityArr: [] // 查看历史城市
    }
  },
  created () {
    this.historyCityArr = this.getHistoryCity();
    this.selectedCity = this.historyCityArr[0].name || this.historyCityArr[0].city;
  },
  methods: {
    // 改变选择城市
    changeCity (newCity) {
      if (this.historyCityArr.length > 2) {
        this.historyCityArr.pop();
      }
      this.historyCityArr.unshift(newCity);
      this.setHistoryCity(this.historyCityArr);
    },
    // 设置历史城市
    setHistoryCity (arr) {
      var historyCityStr = JSON.stringify(arr);
      localStorage.setItem('historyCityArr', historyCityStr);
    },
    // 获取历史城市
    getHistoryCity () {
      let historyCityArr = localStorage.getItem('historyCityArr');
      return historyCityArr ? JSON.parse(historyCityArr) : [];
    }
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
