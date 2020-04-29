<template>
  <view class="content" @click="handleClick">
    <slot ></slot>
  </view>
</template>

<script>
export default {
  props: {
    list: Array,
    index: Number
  },
  methods: {
    handleClick() {
        // 缓存数据全局
        getApp().globalData.imgList=this.list
        getApp().globalData.imgIndex=this.index
        // 实现跳转
        uni.navigateTo({
             url: '/pages/imgDetail/index'
        });
    }
  }
};
</script>

<style lang='scss'>
</style>