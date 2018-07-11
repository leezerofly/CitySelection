<template>
  <div id="app">
    <el-container>
      <el-header>
        <Search :openCityList="openCityList" @change-city="changeCity"/>
        <transition name="flag">
          <div class="nowFlag" v-if="flag">{{flagText}}</div>
        </transition>
      </el-header>
      <scroll :data="openCityList" ref="suggest" :probeType="3" :listenScroll="true" @distance="distance" @isFlagShow="isFlagShow">
        <div>
          <PositionBox :positionCity="positionCity" :historyCityArr="historyCityArr" @change-city="changeCity"/>
          <CityList :hotCityList="hotCityList" :openCityList="openCityList" :elementIndex="elementIndex" @change-city="changeCity" @singleLetter="singleLetter"/>
        </div>
      </scroll>
      <nav-list :navList="cityIndexList" :flagText="flagText" @toElement="toElement"></nav-list>
      <!-- <transition name="flag">
        <div class="nowFlag" v-if="flag">{{flagText}}</div>
      </transition> -->
    </el-container>
  </div>
</template>

<script>
import PositionBox from './components/PositionBox.vue'
import Search from './components/Search.vue'
import CityList from './components/CityList.vue'
import NavList from './components/NavList.vue'
import Scroll from './components/Scroll.vue'

import { getDistance } from './common/js/dom'

import cityData from './cityData'

import axios from 'axios'

export default {
  name: 'app',
  components: {
    PositionBox,
    Search,
    CityList,
    Scroll,
    NavList
  },
  data () {
    return {
      positionCity: {}, // 定位城市
      // selectedCity: {}, // 已选择的城市
      historyCityArr: [], // 查看历史城市
      hotCityList: [], // 热门城市列表
      openCityList: [], // 所有城市列表
      cityIndexList: ['顶'], // 右边导航栏列表
      elementIndex: '', // navlist页点击的index
      arrHeight: [], // 高度数组
      flag: false, // 字母牌是否显示
      flagText: '顶' // 字母牌显示的字
    }
  },
  created () {
    this.getCityListData();
    this.historyCityArr = this.getHistoryCity();
    this.getPosition();
  },
  methods: {
    // 获取城市列表
    getCityListData () {
      this.hotCityList = cityData.hotCityList;
      this.openCityList = cityData.openCityList;
      this.openCityList.map((item) => {
        this.cityIndexList.push(item[0]);
      })
      this.getDomHeight();
    },
    // 定位当前所在城市
    getPosition () {
      var self = this;
      axios.get('https://restapi.amap.com/v3/ip?key=ad0bab010adc9943844643623faa828d')
        .then(function (response) {
          self.positionCity = response.data;
          if (self.positionCity.city !== self.historyCityArr[0].city) {
            self.changeCity(self.positionCity);
          }
        })
        .catch(function (error) {
          self.$message({
            message: `获取定位失败${error}`,
            type: 'warning'
          });
        });
    },
    // 改变选择城市，弹出弹窗
    changeCity (newCity) {
      this.$confirm(`确定切换城市到${newCity.name || newCity.city}?`, {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        center: true,
        showClose: false,
        customClass: 'confirm'
      }).then(() => {
        if (newCity !== this.historyCityArr[0]) {
          if (this.historyCityArr.length > 2) {
            this.historyCityArr.pop();
          }
          this.historyCityArr.unshift(newCity);
          this.setHistoryCity(this.historyCityArr);
          
          // 当确认后滚动到顶部
          this.$refs.suggest.scrollTo(0, 0, 200)
        }
      }).catch(() => {        
      });
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
    },
    // 点击右边nav，向citylist组件传值
    toElement (text) {
      if (text === '顶') {
        this.$refs.suggest.scrollTo(0, 0, 200)
      }
      this.elementIndex = text
    },
    // 根据滑动距离显示字母牌上的字
    distance (val) {
      for (let i = 0, len = this.arrHeight.length; i < len; i++) {
        if (val < this.arrHeight[i]) {
          this.flagText = this.cityIndexList[i]
          return false
        }
      }
    },
    // 计算每一部分到顶端的距离
    getDomHeight () {
      let arr = getDistance(this.openCityList)
      arr.unshift(300) // 向开始添加顶端的250px的距离
      let i = 0
      arr.map((val) => {
        i = i + val
        this.arrHeight.push(i)
      })
    },
    // 是否显示字母牌
    isFlagShow (val) {
      this.flag = val
    },
    // 滚动到相应的dom节点
    singleLetter (dom) {
      this.$refs.suggest.scrollToElement(dom, 200, false, -1)
    },
  }
}
</script>

<style lang="stylus">
body
  margin 0 !important
#app
  font-family "Helvetica Neue", Helvetica , "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", "微软雅黑", Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  color #2c3e50
  .nowFlag
    width 50px
    height 50px
    background #4395ff
    color #fff
    font-size 30px
    font-weight 900
    line-height 50px
    text-align center
    position absolute
    top 60px
    left 50%
    margin-left -25px
  .el-header
    background-color white
    color #333
    font-size 14px
    line-height 60px
    text-align center
    width 100%
    position fixed
    z-index 2000
  
.title
  font-size 18px
    
.confirm
  width 250px !important

.list-enter-active, .list-leave-active
  transition all 0.5s
.list-enter
  opacity 0
.list-leave-to
  transform scale(0, 0)
  opacity 0
.flag-leave-active
  transition all 1s
.flag-leave-to
  opacity 0
</style>
