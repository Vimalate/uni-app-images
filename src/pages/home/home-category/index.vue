<template>
  <view class="content">
    <view class="category">
      <navigator
        class="category_item"
        v-for="(item) in category"
        :key="item.id"
        :url="`/pages/imgCategory/index?id=${item.id}`"
      >
        <image :src="item.cover" mode="aspectFill" />
        <view class="category_name">{{item.name}}</view>
      </navigator>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      category: []
    };
  },
  mounted() {
    uni.setNavigationBarTitle({
      title: "分类"
    });
    this.getList();
  },
  methods: {
    async getList() {
      const res = await this.request({
        url: "https://service.picasso.adesk.com/v1/vertical/category"
      });
      console.log(res);
      this.category = res.res.category;
    }
  }
};
</script>

<style lang='scss' scoped>
.content {
  .category {
    display: flex;
    flex-wrap: wrap;
    .category_item {
      width: 33.3333%;
      position: relative;
      border: 5rpx solid #ffffff;
      image {
        height: 240rpx;
      }

      .category_name {
        position: absolute;
        width: 100%;
        height: 50rpx;
        left: 0;
        bottom: 0;
        color: floralwhite;
        background-image: linear-gradient(
          to right top,
          rgba(0, 0, 0, 0.2),
          rgba(0, 0, 0, 0)
        );
        font-size: 40rpx;
        line-height: 50rpx;
        padding-left: 20rpx;
      }
    }
  }
}
</style>