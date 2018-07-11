<template>
  <div class="position-box">
    <p class="title">你已选择：{{ historyCityArr[0].name || historyCityArr[0].city }}</p>
    <div class="selected-city-history">
      <p class="title">定位</p>
      <el-button size="medium" @click="changeCity(positionCity)">
        <i class="el-icon-location"></i>{{ positionCity.city }}
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
  props: {
    positionCity: Object,
    historyCityArr: Array
  },
  methods: {
    // 发起一个changeCity事件
    changeCity (newCity) {
      this.$emit('change-city', newCity);
    }
  }
}
</script>