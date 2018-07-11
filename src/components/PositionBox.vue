<template>
  <div class="hello">
    <p class="title">你已选择：{{ historyCityArr[0].name || historyCityArr[0].city }}</p>
    <div class="selected-city-history">
      <p class="title">定位</p>
      <el-button size="medium" @click="changeCity(currentCity)">
        <i class="el-icon-location"></i>{{ currentCity.city }}
      </el-button>
      <p class="title">最近选择</p>
      <el-row>
        <el-col v-for="(city, index) in historyCityArr" :key="index" :xs="8" :sm="2" :md="2" :lg="2" :xl="2">
          <el-button size="medium" @click="changeCity(city)">
            {{ city.name || city.city }}
          </el-button>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'PositionBox',
  props: ['selectedCity', 'historyCityArr'],
  data () {
    return {
      currentCity: {}, // 当前所在的城市
    }
  },
  created () {
    this.getPosition();
  },
  methods: {
    // 定位当前所在城市
    getPosition () {
      var self = this;
      axios.get('https://restapi.amap.com/v3/ip?key=ad0bab010adc9943844643623faa828d')
        .then(function (response) {
          self.currentCity = response.data
        })
        .catch(function (error) {
          this.$message({
            message: `获取定位失败${error}`,
            type: 'warning'
          });
        });
    },
    // 发起一个changeCity事件
    changeCity (newCity) {
      this.$emit('change-city', newCity);
    }
  }
}
</script>
