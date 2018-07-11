<template>
  <div class="about">
    <h3>您选择的城市数据为：{{ selectedCity }}</h3>
    <router-link to="/">返回首页</router-link>
  </div>
</template>

<script>
export default {
  data () {
    return {
      selectedCity: {}, // 定位城市
    }
  },
  props: {
    data: {
      type: Array,
      default: null
    },
    // 是否触发滚动时间
    probeType: {
      type: Number,
      default: 1
    },
    // 是否监听滚动事件
    listenScroll: {
      type: Boolean,
      default: false
    }
  },
  created () {
    if (this.$route.params.selectedCity) {
      this.selectedCity = this.$route.params.selectedCity;
    }
  },
}
</script>