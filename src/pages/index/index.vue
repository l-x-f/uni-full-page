<template>
  <full-page :active-index.sync="activeIndex" :total-page="totalPage">
    <view
      class="section"
      v-for="(item, index) in totalPage"
      :key="index"
      :style="getRandomStyle()"
    >
      <div :class="'page page-' + index">
        {{ index + 1 }}
        <button type="primary" @click="toPage(1)">
          跳转到第1页
        </button>
        <button type="primary" @click="toPage(10)">
          跳转到第10页
        </button>
      </div>
    </view>
  </full-page>
</template>

<script>
import FullPage from '@/components/FullPage'
export default {
  components: {
    FullPage,
  },
  data() {
    return {
      totalPage: 10,
      activeIndex: 0,
    }
  },
  methods: {
    getRandomStyle() {
      const r = Math.floor(Math.random() * 256)
      const g = Math.floor(Math.random() * 256)
      const b = Math.floor(Math.random() * 256)
      const color = '#' + r.toString(16) + g.toString(16) + b.toString(16)
      return `background-color:${color}`
    },
    toPage(index) {
      this.activeIndex = index - 1
    },
  },
}
</script>
<style lang="scss" scoped>
page {
  height: 100%;
}
.section {
  height: 100%;
  width: 100%;
  position: relative;
}
.page {
  height: 100%;
  width: 100%;
  text-align: center;
  font-size: 50rpx;
  padding-top: 150rpx;
  box-sizing: border-box;
}
button {
  font-size: 30rpx;
  width: 400rpx;
  margin: 50rpx;
}
</style>
