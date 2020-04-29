<template>
  <view class="content">
    <!-- 用户信息 -->
    <view class="user_info">
      <view class="user_icon">
        <image :src="imgDetail.user.avatar||imgDetail.thumb" mode="widthFix" />
      </view>
      <view class="user_desc">
        <view class="user_name" v-if="imgDetail.user">{{imgDetail.user.name}}</view>
        <view class="user_name" v-else>vimalakirti</view>
        <view class="user_time">{{imgDetail.cnTime}}</view>
      </view>
    </view>
    <!-- 高清大图 -->
    <view class="lg_img">
      <image :src="imgDetail.thumb" mode="widthFix" />
    </view>
    <!-- 点赞 -->
    <view class="user_rank">
      <view class="rank">
        <text class="iconfont icondianzan">{{imgDetail.rank}}</text>
      </view>
      <view class="user_collect">
        <text class="iconfont iconshoucang">收藏</text>
      </view>
    </view>
    <!-- 专辑 -->
    <view class="album_wrap" v-if="album.length">
      <view class="album_title"></view>
      <view class="album_list">
        <view class="album_item" v-for="(item) in album" :key="item.id">
          <view class="album_cover">
            <image :src="item.cover" mode="aspectFill" />
          </view>
          <view class="album_info">
            <view class="album_text">专辑</view>
            <view class="album_name">{{item.name}}</view>
            <text class="iconfont iconiconfontjiantu4"></text>
          </view>
        </view>
      </view>
    </view>
    <!-- 最热评论 -->
    <view class="comment" v-if="hot.length">
      <view class="comment_title">
        <text class="iconfont iconhot1"></text>
        <text class="comment_text">最热评论</text>
      </view>
      <view class="comment_list">
        <view class="comment_item" v-for="(item) in hot" :key="item.id">
          <view class="comment_user">
            <view class="comment_icon">
              <image  :src="item.user.avatar" mode="widthFix" />
            </view>
            <view class="comment_name">
              <view class="user_name">{{item.user.name}}</view>
              <view class="user_time">{{item.cnTime}}</view>
            </view>
            <!-- 徽章 -->
            <view class="user_badge">
              <image v-for="(item2) in item.user.title" :key="item2.icon" :src="item2.icon" mode />
            </view>
          </view>
          <view class="comment_desc">
            <view class="comment_content">{{item.content}}</view>
            <view class="comment_like">
              <text class="iconfont icondianzan">{{item.size}}</text>
            </view>
          </view>
        </view>
      </view>
    </view>
		<!-- 最新评论 -->
		<view class="comment" v-if="comment.length">
      <view class="comment_title">
        <text class="iconfont iconpinglun"></text>
        <text class="comment_text">最新评论</text>
      </view>
      <view class="comment_list">
        <view class="comment_item" v-for="(item) in comment" :key="item.id">
          <view class="comment_user">
            <view class="comment_icon">
              <image  :src="item.user.avatar" mode="widthFix" />
            </view>
            <view class="comment_name">
              <view class="user_name">{{item.user.name}}</view>
              <view class="user_time">{{item.cnTime}}</view>
            </view>
            <!-- 徽章 -->
            <view class="user_badge">
              <image v-for="(item2) in item.user.title" :key="item2.icon" :src="item2.icon" mode />
            </view>
          </view>
          <view class="comment_desc">
            <view class="comment_content">{{item.content}}</view>
            <view class="comment_like">
              <text class="iconfont icondianzan">{{item.size}}</text>
            </view>
          </view>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
import moment, { months } from "moment";
moment.locale("zh-cn");
export default {
  data() {
    return {
      imgDetail: {},
      album: [],
      hot: [],
      comment: []
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
    // this.newThumb =
    //   this.imgDetail.thumb + this.imgDetail.rule.replace("$<Height>", 360);
    // 时间
    this.imgDetail.cnTime = moment(this.imgDetail.atime * 1000).fromNow();
    // 获取图片详情id
    this.getDetail(this.imgDetail.id);
  },
  methods: {
    getDetail(id) {
      this.request({
        url: `https://service.picasso.adesk.com/v2/wallpaper/wallpaper/${id}/comment`
      }).then(result => {
				console.log(result.res);
				// 添加时间属性
				result.res.hot.forEach(v=>v.cnTime=moment(v.atime*1000).fromNow())
				result.res.comment.forEach(v=>v.cnTime=moment(v.atime*1000).fromNow())
        this.album = result.res.album;
        this.hot = result.res.hot;
        this.comment = result.res.comment;
      });
    }
  }
};
</script>

<style lang='scss' scoped>
.content {
  .user_info {
		height: 120rpx;
    display: flex;
    padding: 20rpx;
    .user_icon {
      padding: 0 15rpx;
      image {
        width: 88rpx;
				height: 88rpx;
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
  .user_rank {
    display: flex;
    height: 80rpx;
    border-bottom: 5rpx solid #eee;
    .rank {
      display: flex;
      flex: 1;
      justify-content: center;
      align-items: center;
    }

    .user_collect {
      display: flex;
      flex: 1;
      justify-content: center;
      align-items: center;
    }
  }
  .album_wrap {
    padding: 20rpx;
    .album_title {
      padding: 10rpx 0;
    }

    .album_list {
      .album_item {
        display: flex;
        padding: 10rpx 0;
        border-bottom: 10rpx solid #eee;
        .album_cover {
          flex: 1;
          image {
            height: 80rpx;
            width: 80rpx;
          }
        }

        .album_info {
          padding-left: 20rpx;
          flex: 3;
          position: relative;
          .iconfont {
            font-size: 40rpx;
            position: absolute;
            top: 50%;
            transform: translateY(-50%);
            right: 10%;
            color: black;
          }
          .album_text {
            width: 100rpx;
            height: 50rpx;
            background-color: $color;
            color: #fff;
            line-height: 50rpx;
            text-align: center;
          }

          .album_name {
            color: #888;
            pad: 10rpx 0;
          }
        }
      }
    }
  }
  .comment {
    .comment_title {
      padding: 15rpx;
      text.iconfont.iconhot1 {
        color: red;
        font-size: 40rpx;
      }

      text.comment_text {
        font-weight: 600;
        font-size: 36rpx;
        color: #666;
        margin-left: 10rpx;
      }
    }

    .comment_list {
      .comment_item {
				border-bottom: 10rpx solid #eee;
        .comment_user {
					display: flex;
					padding: 20rpx 0;
          .comment_icon {
						width: 15%;
						display: flex;
						justify-content: center;
						align-items: center;
            image {
							width: 90%;
            }
          }

          .comment_name {
						flex: 1;
						margin-left: 15rpx;
            .user_name {
							color: #777;
            }

            .user_time {
							color: #ccc;
							font-size: 24rpx;
							padding: 5rpx;
            }
          }

          .user_badge {
						margin-right: 20rpx;
            image {
							width: 40rpx;
							height: 40rpx;
            }
          }
        }

        .comment_desc {
					display: flex;
					padding: 30rpx 0;
          .comment_content {
						flex: 1;
						padding-left: 18%;
						color: black;
          }

          .comment_like {
						text-align: right;
            .icondianzan {
							margin-right: 20rpx;
            }
          }
        }
      }
    }
  }
}
.iconpinglun{
	color: aqua!important;
}
.lg_img{
	border-bottom: 1rpx solid #eee;
}
</style>