<template>
  <view class="content">
    <view class="album_bg">
      <image :src="album.cover" mode='widthFix' />
      <view class="album_info">
        <view class="album_name">{{album.name}}</view>
        <view class="album_btn">关注专辑</view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      params: {
        limit: 30,
        order: "new",
        skip: 0,
        first: 1
      },
      id: -1,
      album: {},
      wallpaper: []
    };
  },
  onLoad(options) {
    console.log(options);
    this.id = "5dea510be7bce73981faf1a8";
    this.getList();
  },
  methods: {
    getList() {
      this.request({
        url: `https://service.picasso.adesk.com/v1/wallpaper/album/${this.id}/wallpaper`,
        data: this.params
      }).then(result => {
        console.log(result);
        this.album = result.res.album;
        this.wallpaper = result.res.wallpaper;
      });
    }
  }
};
</script>

<style lang='scss' scoped>
.content {
  .album_bg {
    position: relative;
    image {
    }

    .album_info {
      position: absolute;
      width: 100%;
      left: 0;
      bottom: 0 ;
      display: flex;
      justify-content: space-between;
      height: 80rpx;
      align-items: center;
      color: #fff;
      padding: 0 15rpx ;
      .album_name {
          font-size: 40rpx;
      }

      .album_btn {
          background-color: $color;
          width: 152rpx;
          height: 60rpx;
          border-radius: 10rpx;
          display: flex;
          align-items: center;
          justify-content: center;
      }
    }
  }
}
</style>