<template>
  <view class="content" v-if="recommends.length>0">
    <!-- 推荐 -->
    <view class="recommend_wrap">
      <view class="recommend_item" v-for="(item) in recommends" :key="item.id">
        <img :src="item.thumb" mode="widthFix" alt />
      </view>
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
        <view class="month_item" v-for="(item) in monthes.items" :key="item.id">
          <img mode="aspectFill" :src="item.thumb+item.rule.replace('$<Height>',360)" alt />
        </view>
      </view>
    </view>
  </view>
</template>

<script>
import moment from "moment";
export default {
  data() {
    return {
      // 推荐
      recommends: [],
      monthes: []
    };
  },
  mounted() {
    this.getList();
  },
  methods: {
    getList() {
      this.request({
        url: "http://service.picasso.adesk.com/v3/homepage/vertical",
        data: {
          limit: 30,
          order: "hot",
          skip: 0
        }
      }).then(res => {
        console.log(res);
        // 推荐
        this.recommends = res.res.homepage[1].items;
        // 月份
        this.monthes = res.res.homepage[2];
        this.monthes.MM = moment(this.monthes.stime).format("MM");
        this.monthes.DD = moment(this.monthes.stime).format("DD");
      });
    }
  }
};
</script>

<style lang='scss'>
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
</style>