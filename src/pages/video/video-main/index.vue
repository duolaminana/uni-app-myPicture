<template>
  <scroll-view scroll-y enable-flex class="video_main" @scrolltolower="handleScrolltolower">
    <view class="main_item" v-for="item in videowp" :key="item.id" @click="handleGoVideo(item)">
      <image :src="item.img" mode="widthFix" />
    </view>
  </scroll-view>
</template>

<script>
export default {
  props: {
    obj: Object
  },
  watch: {
    obj() {
      console.log(this.obj);
      this.videowp=[]
      this.getData();
    }
  },
  data() {
    return {
      videowp: [],
      hasMore: true
    };
  },
  mounted() {
    console.log(this.obj);
    this.getData();
  },
  methods: {
    handleGoVideo(value){
      getApp().globalData.video=value
      // 页面跳转
      uni.navigateTo({
        url:"/pages/videoPlay/index"
      })
    },
    handleScrolltolower() {
      if (this.hasMore) {
        this.obj.params.skip += this.obj.params.limit;
        this.getData();
      } else {
        uni.showToast({
          title: "没有更多数据了",
          icon: "none"
        });
      }
    },
    getData() {
      this.request({
        url: this.obj.url, //开发者服务器接口地址",
        data: this.obj.params //请求的参数",
      }).then(result => {
        console.log(result);
        // 判断有没有下一页数据
        if (result.res.videowp.length == 0) {
          this.hasMore = false;
          uni.showToast({
            title: "没有更多数据了",
            icon: "none"
          });
          return;
        }

        this.videowp = [...this.videowp, ...result.res.videowp];
      });
    }
  }
};
</script>

<style lang="scss" scoped>
.video_main {
  display: flex;
  flex-wrap: wrap;
  height: calc(100vh - 36rpx);
  .main_item {
    width: 33.33%;
    border: 5rpx solid #fff;
  }
}
</style>