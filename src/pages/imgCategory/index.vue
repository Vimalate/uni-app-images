<template>
  <view class="category">
    <view class="category_tab">
      <view class="category_tab_title">
        <view class="category_tab_inner">
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
        <scroll-view
          enable-flex
          @scrolltolower="handlescroll"
          scroll-y
          class="category_tab_content"
        >
          <view class="cate_item" v-for="(item,index) in vertical" :key="item.id">
						 <go-detail :list='vertical' :index='index'>
            <image :src="item.thumb" mode="widthFix" />
						 </go-detail>
          </view>
          <!-- <view v-if="current === 0">
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
          </view>-->
        </scroll-view>
    </view>
  </view>
</template>

<script>
import { uniSegmentedControl } from "@dcloudio/uni-ui";
import goDetail from "@/components/goDetail";
export default {
	components:{
		goDetail,
		uniSegmentedControl
	},
  data() {
    return {
      items: [
        { title: "最新", order: "new" },
        { title: "热门", order: "hot" }
      ],
      current: 0,
      params: {
        limit: 30,
        skip: 0,
        order: "new"
      },
      id: 0,
      vertical: [],
      hasMore: true
    };
  },
  onLoad(options) {
    this.id = options.id;
    this.getList();
  },
  methods: {
    handlescroll() {
      if (this.hasMore) {
        this.params.skip += this.params.limit;
        this.getList();
      } else {
        uni.showToast({
          title: "没有数据了",
          icon: "none",
          duration: 2000
        });
      }
    },
    onClickItem(e) {
      // 切换标题
      if (this.current !== e.currentIndex) {
        this.current = e.currentIndex;
      } else {
        return;
      }
      // // 切换 tab skip重置
      // this.params.skip = 0;
      // this.vertical = [];
      //   切换 order
      this.params.order = this.items[e.currentIndex].order;
      // 切换 tab skip重置
      this.params.skip=0
      this.vertical=[]
      // 重新发送请求
      this.getList();
    },
    async getList() {
      const res = await this.request({
        url: `https://service.picasso.adesk.com/v1/vertical/category/${this.id}/vertical`,
        data: this.params
      });
      console.log(res);
      if (res.res.vertical.length === 0) {
        this.hasMore = false;
        uni.showToast({
          title: "没有数据了",
          icon: "none",
          duration: 2000
        });
        return;
      }
      this.vertical = [...this.vertical, ...res.res.vertical];

      console.log(this.vertical);
    }
  },
  // components: {
    // homeAlbum,
    // homeRecommend,
    // homeCategory,
    
  // }  // homeNew,
  
};
</script>

<style lang='scss' scoped>
.category_tab {
  .category_tab_title {
    display: flex;
    justify-content: space-around;
    align-items: center;
    .category_tab_inner {
      width: 70vw;
    }
  }
}
.category_tab_content {
  display: flex;
  flex-wrap: wrap;
  height: calc(100vh - 36px);
  .cate_item {
    width: 33.333%;
    border: 5rpx solid #fff;
    image {
    }
  }
}
</style>