<template>
  <view class="home">
    <home-album></home-album>
    <home-recommend></home-recommend>
    <home-new></home-new>
    <home-category></home-category>
  </view>
</template>

<script>
import homeAlbum from './home-album'
import homeRecommend from './home-recommend'
import homeNew from './home-new'
import homeCategory from './home-category'
import {uniSegmentedControl} from '@dcloudio/uni-ui'
export default{
  components:{
    homeAlbum,
    homeRecommend,
    homeNew,
    homeCategory
  }
}
</script>

<style lang='scss'>
</style>