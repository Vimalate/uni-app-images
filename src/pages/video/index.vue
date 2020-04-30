<template>
  <view class="video">
    <view class="video_tab">
      <view class="video_tab_title">
        <view class="video_tab_inner">
          <uni-segmented-control
            :current="current"
            :values="items.map(v=>v.title)"
            @clickItem="onClickItem"
            style-type="text"
            active-color="#d4237a"
          ></uni-segmented-control>
        </view>
        <view class="iconfont iconsearch"></view>
      </view>
      <view class="video_tab_content">
        <view v-if="current<4">
          <video-main></video-main>
        </view>
        <view v-if="current===4">
          <video-category></video-category>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
import { uniSegmentedControl } from "@dcloudio/uni-ui";
import videoMain from "./videoMain";
import videoCategory from "./videoCategory";
export default {
  data() {
    //   https://service.picasso.adesk.com/v2/wallpaper/wallpaper/${id}/comment
    // http://157.122.54.189:9088/videoimg/v1/videowp/category
    //  https://service.picasso.adesk.com/videoimg/v1/videowp/category
    //  https://service.picasso.adesk.com/v1/videowp/category
    return {
      items: [
        {
          title: "推荐",
          url: "http://157.122.54.189:9088/videoimg/v1/videowp/featured",
          params: { limit: 30, skip: 0, order: "hot" }
        },
        {
          title: "娱乐",
          url:
            "http://157.122.54.189:9088/videoimg/v1/videowp/category/59b25abbe7bce76bc834198a",
          params: { limit: 30, skip: 0, order: "new" }
        },
        {
          title: "最新",
          url: "http://157.122.54.189:9088/videoimg/v1/videowp/videowp",
          params: { limit: 30, skip: 0, order: "new" }
        },
        {
          title: "热门",
          url: "http://157.122.54.189:9088/videoimg/v1/videowp/videowp",
          params: { limit: 30, skip: 0, order: "hot" }
        },
        {
          title: "分类",
          url: "http://157.122.54.189:9088/videoimg/v1/videowp/category",
          params: {}
        }
      ],
      current: 0
    };
  },
  methods: {
    onClickItem(e) {
      if (this.current !== e.currentIndex) {
        this.current = e.currentIndex;
      }
    }
  },
  components: {
    uniSegmentedControl,
    videoMain,
    videoCategory
  }
};
</script>

<style lang='scss' scoped>
.video_tab {
  .video_tab_title {
    display: flex;
    justify-content: space-around;
    align-items: center;
    .video_tab_inner {
      width: 70vw;
    }
  }
}
</style>