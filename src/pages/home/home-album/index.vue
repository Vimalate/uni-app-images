<template>
  <scroll-view class="scroll-view" scroll-y @scrolltolower="handleToLower">
    <!-- 轮播图 -->
    <view class="swiper_wrap">
      <swiper class="swiper" autoplay indicator-dots circular>
        <swiper-item v-for="item in banner" :key="item.id">
          <img :src="item.thumb" alt />
        </swiper-item>
      </swiper>
    </view>
    <!-- 专辑 -->
    <view class="ablum_list">
      <navigator :url="`/pages/album/index?id=${item.id}`" class="ablum_item" v-for="item in album" :key="item.id">
        <view class="ablum_img">
          <image mode="aspectFill" :src="item.cover" alt />
        </view>
        <view class="abum_info">
          <view class="ablum_name">{{item.name}}</view>
          <view class="ablum_desc">{{item.desc}}</view>
          <view class="ablum_btn">
            <view class="ablum_attention">关注</view>
          </view>
        </view>
      </navigator>
    </view>
  </scroll-view>
</template>

<script>
export default {
  data() {
    return {
      params: {
        lmit: 30,
        order: "new",
        skip: 0
      },
      banner: [],
      album: [],
      hasMore: true
    };
  },
  mounted() {
    uni.setNavigationBarTitle({
      title: "专辑"
    });
    this.getList();
  },
  methods: {
    getList() {
      this.request({
        url: "https://service.picasso.adesk.com/v1/wallpaper/album",
        data: this.params
      }).then(result => {
        if (this.banner.length === 0) {
          this.banner = result.res.banner;
        }
        if (result.res.album.length === 0) {
          this.hasMore = false;
          return;
        }
        this.album = [...this.album, ...result.res.album];
      });
    },
    handleToLower() {
      console.log(111);
      if (this.hasMore) {
        this.params.skip += this.params.lmit;
        this.getList();
      } else {
        uni.showToast({
          title: "没有数据le",
          icon: "none",
          duration: 2000
        });
      }
    }
  }
};
</script>

<style lang='scss' scoped>
.scroll-view {
  height: calc(100vh - 36px);
}
.swiper_wrap {
  height: 326rpx;
  .swiper {
    height: 100%;
  }
  image {
    height: 100%;
  }
}
.ablum_list {
  padding: 10rpx;
  .ablum_item {
    display: flex;
    padding: 10rpx 0;
    border-bottom: 1rpx solid #ccc;
    .ablum_img {
      flex: 1;
      padding: 10rpx;
      image {
        width: 200rpx;
        height: 200rpx;
      }
    }

    .abum_info {
      flex: 2;
      padding: 0 10rpx;
      overflow: hidden;
      .ablum_name {
        font-size: 30rpx;
        color: #000;
        padding: 10rpx 0;
      }

      .ablum_desc {
        padding: 10rpx 0;
        font-size: 24rpx;
        text-overflow: ellipsis;
        overflow: hidden;
        white-space: nowrap;
      }

      .ablum_btn {
        padding: 10rpx;
        display: flex;
        justify-content: flex-end;
        .ablum_attention {
          color: $color;
          border: 1rpx solid $color;
          padding: 10rpx;
        }
      }
    }
  }
}
</style>