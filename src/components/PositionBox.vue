<template>
  <div class="hello">
    <div class="selected-city">
      <span>你已选择：{{ historyCityArr[0].name || historyCityArr[0].city }}</span>
    </div>
    <div class="selected-city-history">
      <h3>定位</h3>
      <el-button size="medium" @click="changeCity(currentCity)">
        <i class="el-icon-location"></i>{{ currentCity.city }}
      </el-button>
      <h3>最近选择</h3>
      <el-row>
        <el-col v-for="city in historyCityArr" :key="city.id" :xs="8" :sm="2" :md="2" :lg="2" :xl="2">
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
    this.getPosition()
  },
  methods: {
    // 定位当前所在城市
    getPosition () {
      var self = this
      axios.get('https://restapi.amap.com/v3/ip?key=ad0bab010adc9943844643623faa828d')
        .then(function (response) {
          self.currentCity = response.data
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    // 发起一个changeCity事件
    changeCity (newCity) {
      this.$emit('change-city', newCity)
    }
  }
}
</script>
