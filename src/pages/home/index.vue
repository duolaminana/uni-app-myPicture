<template>
  <view>
    <view class="home_tab">
      <view class="home_tab_title">
        <view class="title-inner">
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
        <view class="content">
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
  </view>
</template>

<script>
import homeAlbum from "./home-album";
import homeCategory from "./home-category";
import homeNew from "./home-new";
import homeRecommend from "./home-recommend";
import { uniSegmentedControl } from "@dcloudio/uni-ui";
export default {
  components: {
    homeAlbum,
    homeCategory,
    homeNew,
    homeRecommend,
    uniSegmentedControl
  },
  onLoad() {
    this.request({
      url: 'http://157.122.54.189:9088/image/v3/homepage/vertical', //开发者服务器接口地址",
    })
    .then(res=>{
      
    })
  },
  data() {
    return {
      items: [
        { title: "推荐" },
        { title: "分类" },
        { title: "最新" },
        { title: "专辑" }
      ],
      current: 1
    };
  },
  methods: {
    onClickItem(e) {
      if (this.current !== e.currentIndex) {
        this.current = e.currentIndex;
      }
    }
  }
};
</script>

<style lang="scss">
  .home_tab{
    .home_tab_title{
      position: relative;
      .title-inner{
        width: 60%;
        margin: 0 auto;
      }
      .iconsearch{
        position: absolute;
        right: 5%;
        top: 50%;
        transform: translateY(-50%);

      }
    }
  }
</style>