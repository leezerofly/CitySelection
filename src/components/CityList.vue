<template>
  <div>
    <p class="title">热门城市</p>
    <el-row>
      <el-col v-for="city in hotCityList" :key="city.id" :xs="8" :sm="2" :md="2" :lg="2" :xl="2">
        <el-button size="medium" @click="changeCity(city)">{{ city.name }}</el-button>
      </el-col>
    </el-row>
    <div v-for="citys in openCityList" :key="citys[0]">
      <p class="title" :ref="citys[0]">{{ citys[0] }}</p>
      <el-table
        :data="citys[1]"
        :show-header="false"
        highlight-current-row
        @current-change="changeCity"
        style="width: 100%">
        <el-table-column prop="name"></el-table-column>
      </el-table>
    </div>
  </div>
</template>

<script>

export default {
  props: {
    hotCityList: {
      type: Array,
      default: []
    },
    openCityList: {
      type: Array,
      default: []
    },
    // cityIndexList: Array,
    elementIndex: String
  },
  methods: {
    // 发起一个changeCity事件
    changeCity (newCity) {
      this.$emit('change-city', newCity);
    }
  },
  watch: {
    elementIndex (val) {
      if (val === '顶') {
        return false;
      }
      this.$emit('singleLetter', this.$refs[val][0]);
    }
  }
}
</script>