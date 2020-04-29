<template>
  <view class="content">
    <view class="album_bg">
      <image :src="album.cover" mode="widthFix" />
      <view class="album_info">
        <view class="album_name">{{album.name}}</view>
        <view class="album_btn">关注专辑</view>
      </view>
    </view>
    <!-- 专辑作者 -->
    <view class="albm_author">
      <view class="aalbum_info">
        <image :src="album.user.avatar" mode="widthFix" alt />
        <view class="album_name">{{album.user.name}}</view>
      </view>
      <view class="aalbum_desc">
        <text>{{album.desc}}</text>
      </view>
    </view>
    <!-- 专辑列表 -->
    <view class="album_list">
      <view class="album_item" v-for="(item,index) in wallpaper" :key="item.id">
        <go-detail :list='wallpaper' :index='index'>
          <image :src="item.thumb+item.rule.replace('$<Height>',360)" mode="widthFix" />
        </go-detail>
      </view>
    </view>
  </view>
</template>

<script>
import goDetail from "@/components/goDetail";
export default {
  components: {
    goDetail
  },
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
      wallpaper: [],
      hasMore: true
    };
  },
  onLoad(options) {
    console.log(options);
    this.id = options.id;
    // this.id = '5dde7512e7bce7396550fd4f';
    this.getList();
  },
  onReachBottom() {
    if (this.hasMore) {
      // 不是第一次请求
      this.params.first = 0;
      this.params.skip += this.params.limit;
      this.getList();
    } else {
      uni.showToast({
        title: "没有更多了",
        icon: "none",
        duration: 2000
      });
    }
  },
  methods: {
    getList() {
      this.request({
        url: `https://service.picasso.adesk.com/v1/wallpaper/album/${this.id}/wallpaper`,
        data: this.params
      }).then(result => {
        console.log(result);
        if (Object.keys(this.album).length === 0) {
          this.album = result.res.album;
        }
        if (result.res.wallpaper.length === 0) {
          this.hasMore = false;
          return;
        }
        this.wallpaper = [...this.wallpaper, ...result.res.wallpaper];
      });
    }
  }
};
</script>

<style lang='scss' scoped>
.content {
  .album_bg {
    position: relative;
    

    .album_info {
      position: absolute;
      width: 100%;
      left: 0;
      bottom: 0;
      display: flex;
      justify-content: space-between;
      height: 80rpx;
      align-items: center;
      color: #fff;
      padding: 0 15rpx;
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
  .albm_author {
    padding: 10rpx;
    .aalbum_info {
      display: flex;
      padding: 10rpx 0;
      image {
        width: 60rpx;
      }

      .album_name {
        color: #000;
        margin-left: 15rpx;
      }
    }

  }
  .album_list {
    display: flex;
    flex-wrap: wrap;
    .album_item {
      width: 33.33%;
      border: 3rpx solid #fff;
    }
  }
}
</style>