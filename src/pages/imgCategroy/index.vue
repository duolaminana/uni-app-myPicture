<template>
  <view>
    <view class="cate_tab">
      <view class="cate_tab_title">
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
      <scroll-view
        enable-flex
        scroll-y
        class="cate_tab_content"
        @scrolltolower="handleScrolltolower"
      >
        <view class="cate_item" v-for="(item,index) in vertical" :key="item.id">
          <image @click="handleDownload(item.thumb)" :src="item.thumb" mode="widthFix" />
        </view>
      </scroll-view>
    </view>
  </view>
</template>

<script>
import { uniSegmentedControl } from "@dcloudio/uni-ui";
export default {
  components: {
    uniSegmentedControl
  },
  data() {
    return {
      items: [{ title: "最新" }, { title: "热门" }],
      current: 0,
      params: {
        limit: 30,
        skip: 0,
        order: "new"
      },
      vertical: [],
      hasMore: true,
      id: null
    };
  },
  onLoad(e) {
    console.log(e);
    this.id = e.id;
    this.getData();
  },
  methods: {
    async handleDownload(img) {
      uni.showLoading({ title: "下载中" });
      // 远程下载到内存中
      const result1 = await uni.downloadFile({ url: img });
      console.log(result1);
      const { tempFilePath } = result1[1];
      console.log(tempFilePath);
      
      // 内存文件添加到本地
      const result2 = await uni.saveImageToPhotosAlbum({
        filePath: tempFilePath
      });
      console.log(result2);

      uni.hideLoading(),
        uni.showToast({
          title: "下载成功"
        });
    },
    handleScrolltolower() {
      if (this.hasMore) {
        this.params.skip += this.params.limit;
        this.getData();
      } else {
        uni.showToast({
          title: "没有更多数据了",
          icon: "none"
        });
      }
    },
    onClickItem(e) {
      if (this.current !== e.currentIndex) {
        this.current = e.currentIndex;
      }
      this.current ? (this.params.order = "hot") : (this.params.order = "new");
      this.params.skip = 0;
      this.vertical = [];
      this.hasMore = true;
      this.getData();
    },

    getData() {
      this.request({
        url: `http://157.122.54.189:9088/image/v1/vertical/category/${this.id}/vertical`,
        data: this.params
      }).then(result => {
        console.log(result);

        // 判断有没有下一页数据
        if (result.res.vertical.length == 0) {
          this.hasMore = false;
          uni.showToast({
            title: "没有更多数据了",
            icon: "none"
          });
          return;
        }
        this.vertical = [...this.vertical, ...result.res.vertical];
      });
    }
  }
};
</script>

<style lang="scss">
.cate_tab {
  .cate_tab_title {
    position: relative;
    .title-inner {
      width: 60%;
      margin: 0 auto;
    }
    .iconsearch {
      position: absolute;
      right: 5%;
      top: 50%;
      transform: translateY(-50%);
    }
  }
  .cate_tab_content {
    display: flex;
    flex-wrap: wrap;
    height: calc(100vh - 36px);
    .cate_item {
      width: 33.33%;
      border: 5rpx solid #fff;
    }
  }
}
</style>