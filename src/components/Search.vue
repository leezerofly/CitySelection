<template>
  <el-row :span="24" class="demo-autocomplete" >
    <el-col :span="24">
      <el-autocomplete
        class="inline-input"
        v-model="state2"
        :fetch-suggestions="querySearch"
        value-key="name"
        placeholder="输入城市名搜索"
        :trigger-on-focus="false"
        @select="handleSelect"
        prefix-icon="el-icon-search"
        style="width: 100%;"
      ></el-autocomplete>
    </el-col>
  </el-row>
</template>

<script>
import cityData from '../cityData.js'

export default {
  data() {
    return {
      searchList: [],
      state1: '',
      state2: ''
    };
  },
  methods: {
    // 搜索函数，调用过滤条件函数过滤，并将结果处理传给回调函数
    querySearch(queryString, callback) {
      var searchList = this.searchList;
      var results = queryString ? searchList.filter(this.createFilter(queryString)) : searchList;
      // 调用 callback 返回建议列表的数据
      callback(results);
    },
    // 过滤条件函数，根据汉字或拼音过滤
    createFilter(queryString) {
      return (searchItem) => {
        return (searchItem.name.toLowerCase().indexOf(queryString.toLowerCase()) === 0 || 
          searchItem.pinyin.toLowerCase().indexOf(queryString.toLowerCase()) === 0);
      };
    },
    // 处理并加载搜索列表
    loadAll() {
      var searchList = [];
      var openCityList = cityData.openCityList;
      openCityList.map(function (item) {
        searchList = searchList.concat(item[1]);
      })
      return searchList;
    },
    // 发起一个changeCity事件
    handleSelect(newCity) {
      this.$emit('change-city', newCity)
    }
  },
  mounted() {
    this.searchList = this.loadAll();
  }
}
</script>
