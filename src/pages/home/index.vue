<template>
  <view class="home">
    <view class="home_tab">
      <view class="home_tab_title">
        <view class="home_tab_inner">
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
      <view class="home_tab_content">
        <view v-if="current === 0">
          <home-recommend></home-recommend>
        </view>
        <view v-if="current === 1">
          <home-category></home-category>
        </view>
        <view v-if="current === 2">
          <home-new></home-new>
        </view>
        <view v-if="current === 3">
          <home-album></home-album>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
import homeAlbum from "./home-album";
import homeRecommend from "./home-recommend";
import homeNew from "./home-new";
import homeCategory from "./home-category";
import { uniSegmentedControl } from "@dcloudio/uni-ui";
export default {
  data() {
    return {
      items: [
        { title: "推荐" },
        { title: "分类" },
        { title: "最新" },
        { title: "专辑" }
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
    homeAlbum,
    homeRecommend,
    homeNew,
    homeCategory,
    uniSegmentedControl
  },
  
};
</script>

<style lang='scss'>
.home_tab {
  .home_tab_title {
    display: flex;
    justify-content: space-around;
    align-items: center;
    .home_tab_inner {
      width: 70vw;
    }
    .iconsearch {
    }
  }
}
</style>