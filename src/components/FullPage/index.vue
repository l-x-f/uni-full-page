<template>
  <view class="full-page-container">
    <view
      class="full-page-main"
      @touchstart="handleTouchStart"
      @touchmove="handleTouchMove"
      @touchend="handleTouchEnd"
      :style="style"
    >
      <slot />
    </view>
  </view>
</template>

<script>
export default {
  name: 'FullPage',
  props: {
    // 触发翻页的临界值
    critical: {
      type: Number,
      default: 50
    },
    // 总共页面数
    totalPage: {
      type: Number,
      required: true,
      default: 0
    },
    // 当前页面的索引值
    activeIndex: {
      type: Number,
      required: true,
      default: 0
    }
  },
  data() {
    return {
      pageIndex: 0, // 当前页面的索引值
      startPageY: 0, // 开始的位置
      endPageY: 0, // 结束的位置
      marginTop: 0 // 滑动下拉(上拉)距离
    }
  },
  mounted() {
    this.pageIndex = this.activeIndex
  },
  computed: {
    style() {
      return `transform:translateY(-${this.pageIndex * 100}%);margin-top:${
        this.marginTop
      }px`
    }
  },
  watch: {
    activeIndex(value) {
      this.pageIndex = value
    }
  },
  methods: {
    // 开始滑动
    handleTouchStart(e) {
      const { pageY } = e.touches[0]
      this.startPageY = pageY
    },
    // 滑动中
    handleTouchMove(e) {
      const { pageY } = e.touches[0]
      if (
        pageY - this.startPageY < this.critical &&
        pageY - this.startPageY > -this.critical
      ) {
        this.marginTop = pageY - this.startPageY
      }
      this.endPageY = pageY
    },
    // 滑动结束
    handleTouchEnd() {
      if (!this.endPageY) {
        return
      }
      if (
        this.endPageY - this.startPageY > this.critical &&
        this.pageIndex > 0
      ) {
        this.pageIndex -= 1
      } else if (
        this.endPageY - this.startPageY < -this.critical &&
        this.pageIndex < this.totalPage - 1
      ) {
        this.pageIndex += 1
      }
      this.$emit('update:activeIndex', this.pageIndex)
      this.startPageY = 0
      this.endPageY = 0
      this.marginTop = 0
    }
  }
}
</script>

<style lang="scss" scoped>
.full-page-container {
  height: 100%;
  overflow: hidden;
  .full-page-main {
    height: 100%;
    transition: all 0.3s;
  }
}
</style>
