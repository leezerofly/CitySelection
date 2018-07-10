<template>
  <el-row :span="24" class="demo-autocomplete" >
    <el-col :span="24">
      <el-autocomplete
        class="inline-input"
        v-model="state2"
        :fetch-suggestions="querySearch"
        placeholder="输入城市名搜索"
        :trigger-on-focus="false"
        @select="handleSelect"
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
    querySearch(queryString, callback) {
      var searchList = this.searchList;
      var results = queryString ? searchList.filter(this.createFilter(queryString)) : searchList;
      var resultsList = [];
      results.map(function (result) {
        resultsList.push({value:result.name})
      })
      // 调用 callback 返回建议列表的数据
      callback(resultsList);
    },
    createFilter(queryString) {
      return (searchItem) => {
        return (searchItem.name.toLowerCase().indexOf(queryString.toLowerCase()) === 0 || 
          searchItem.pinyin.toLowerCase().indexOf(queryString.toLowerCase()) === 0);
      };
    },
    loadAll() {
      var searchList = [];
      var openCityList = cityData.openCityList;
      openCityList.map(function (item) {
        searchList = searchList.concat(item[1]);
      })
      return searchList;
    },
    handleSelect(item) {
      // console.log(item);
    }
  },
  mounted() {
    this.searchList = this.loadAll();
  }
}
</script>
