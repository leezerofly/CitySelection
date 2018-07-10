<template>
  <div>
    <p>热门城市</p>
    <el-row>
      <el-col v-for="city in hotCityList" :key="city.id" :xs="8" :sm="2" :md="2" :lg="2" :xl="2">
        <el-button size="medium">{{ city.name }}</el-button>
      </el-col>
    </el-row>
    <div v-for="city in openCityList" :key="city[0]">
      <h3>{{ city[0] }}</h3>
      <el-table
        :data="city[1]"
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
      hotCityList: cityData.hotCityList,
      openCityList: cityData.openCityList
    }
  }
}
</script>