<template>
  <div>
    <p class="title">热门城市</p>
    <el-row>
      <el-col v-for="city in hotCityList" :key="city.id" :xs="8" :sm="2" :md="2" :lg="2" :xl="2">
        <el-button size="medium" @click="changeCity(city)">{{ city.name }}</el-button>
      </el-col>
    </el-row>
    <div v-for="city in openCityList" :key="city[0]">
      <p class="title">{{ city[0] }}</p>
      <el-table
        :data="city[1]"
        :show-header="false"
        highlight-current-row
        @current-change="handleCurrentChange"
        style="width: 100%">
        <el-table-column prop="name"></el-table-column>
      </el-table>
    </div>
  </div>
</template>

<script>
import cityData from '../cityData.js'

export default {
  data() {
    return {
      hotCityList: cityData.hotCityList, // 热门城市列表
      openCityList: cityData.openCityList // 城市列表
    }
  },
  methods: {
    // 通过Element的table单选发起一个changeCity事件
    handleCurrentChange(newCity) {
      this.$emit('change-city', newCity);
    },
    // 通过按钮发起一个changeCity事件
    changeCity (newCity) {
      this.$emit('change-city', newCity);
    }
  }
}
</script>