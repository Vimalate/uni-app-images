<template>
  <view class="content">
    <!-- 用户信息 -->
    <view class="user_info">
      <view class="user_icon">
        <image :src="imgDetail.user.avatar" mode="widthFix" />
      </view>
      <view class="user_desc">
        <view class="user_name">{{imgDetail.user.name}}</view>
        <view class="user_time">{{imgDetail.atime}}</view>
      </view>
    </view>
    <!-- 高清大图 -->
    <view class="lg_img">
      <image :src="newThumb" mode="widthFix" />
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      imgDetail: {},
      newThumb: ""
    };
  },
  mounted() {
    uni.setNavigationBarTitle({
      title: "图片详情"
    });
  },
  onLoad() {
    console.log(getApp().globalData);
    const { imgList, imgIndex } = getApp().globalData;
    this.imgDetail = imgList[imgIndex];
    this.newThumb =
      this.imgDetail.thumb + this.imgDetail.rule.replace("$<Height>", 360);
  }
};
</script>

<style lang='scss' scoped>
.content {
  .user_info {
    display: flex;
    padding: 20rpx;
    .user_icon {
      padding: 0 20rpx;
      image {
        width: 88rpx;
        border-radius: 50%;
      }
    }

    .user_desc {
      .user_name {
        color: black;
        font-weight: 600;
      }

      .user_time {
        font-size: 24rpx;
        color: #ccc;
        padding: 10rpx 0;
      }
    }
  }
}
</style>