<template>
  <scroll-view @scrolltolower="handleTolower" scroll-y class="content" v-if="recommends.length>0">
    <!-- 推荐 -->
    <view class="recommend_wrap">
      <navigator
        :url="`/pages/album/index?id=${item.target}`"
        class="recommend_item"
        v-for="(item) in recommends"
        :key="item.id"
      >
        <img :src="item.thumb" mode="widthFix" alt />
      </navigator>
    </view>
    <!-- 日期 -->
    <view class="month_wrap">
      <view class="month_title">
        <view class="month_title_info">
          <view class="month_title">
            <text>{{monthes.DD}} /</text>
            {{monthes.MM}}月
          </view>
          <view class="month_text">清风拂晓，是夏</view>
        </view>
        <view class="month_title_more">更多 ></view>
      </view>
      <view class="month_content">
        <view class="month_item" v-for="(item,index) in monthes.items" :key="item.id">
          <go-detail :list='monthes.items' :index='index'>
          <image mode="aspectFill" :src="item.thumb+item.rule.replace('$<Height>',360)" alt />
            </go-detail>
        </view>
      </view>
    </view>
    <!-- 热门 -->
    <view class="hots_wrap">
      <view class="hots_title">
        <text>热门</text>
      </view>
      <view class="hots_content">
        <view class="hots_item" v-for="(item,index) in hots" :key="item.id">
          <go-detail :list='hots' :index='index'>
          <image mode="widthFix" :src="item.thumb" alt />
          </go-detail>
        </view>
      </view>
    </view>
  </scroll-view>
</template>

<script>
import moment from "moment";
import goDetail from "@/components/goDetail";
export default {
  data() {
    return {
      // 推荐
      recommends: [],
      monthes: [],
      hots: [],
      // 请求参数
      params: {
        limit: 30,
        order: "hot",
        skip: 0
      },
      // 是否还有下一页
      hasMore: true
    };
  },
  components: {
    goDetail
  },
  mounted() {
    this.getList();
  },
  methods: {
    getList() {
      this.request({
        url: "http://service.picasso.adesk.com/v3/homepage/vertical",
        data: this.params
      }).then(res => {
        console.log(res);
        // 判断是否还有下一页数据
        if (res.res.vertical.length === 0) {
          this.hasMore = uni.showToast({
            title: "没有更多了",
            icon: "none",
            duration: 2000
          });
          return;
        }
        // 第一次发请求
        if (this.recommends.length === 0) {
          // 推荐
          this.recommends = res.res.homepage[1].items;
          // 月份
          this.monthes = res.res.homepage[2];
          this.monthes.MM = moment(this.monthes.stime).format("MM");
          this.monthes.DD = moment(this.monthes.stime).format("DD");
        }

        // 热门
        // 数组拼接
        this.hots = [...this.hots, ...res.res.vertical];
      });
    },
    // 滚动条触底事件
    handleTolower() {
      console.log("ok");
      if (this.hasMore) {
        this.params.skip += this.params.limit;
        this.getList();
      } else {
        uni.showToast({
          title: "没有更多了",
          icon: "none",
          duration: 2000
        });
      }
    }
  }
};
</script>

<style lang='scss'>
.content {
  height: calc(100vh - 36px);
}
.recommend_wrap {
  display: flex;
  flex-wrap: wrap;
  .recommend_item {
    width: 50vw;
    border: 5rpx solid #fff;
  }
}
.month_wrap {
  .month_title {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10rpx;
    .month_title_info {
      display: flex;
      align-items: center;
      color: $color;
      font-size: 30rpx;
      font-weight: 600;
      .month_title {
        text {
          font-size: 36rpx;
        }
      }

      .month_text {
        font-size: 34rpx;
        color: #666;
        margin-left: 30rpx;
      }
    }

    .month_title_more {
      font-size: 24rpx;
      color: $color;
    }
  }

  .month_content {
    display: flex;
    flex-wrap: wrap;
    .month_item {
      width: 33.333%;
      border: 3rpx solid #fff;
    }
  }
}
.hots_wrap {
  .hots_title {
    padding: 20rpx;
    text {
      padding: 20rpx;
      border-left: 20rpx solid $color;
      font-size: 34rpx;
      font-weight: 600;
    }
  }

  .hots_content {
    display: flex;
    flex-wrap: wrap;
    .hots_item {
      width: 33.333%;
      border: 3rpx solid #fff;
      image {
      }
    }
  }
}
</style>