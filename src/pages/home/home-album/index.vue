<template>
  <scroll-view class="album_scroll_view" scroll-y @scrolltolower="handleToLower">
    <!-- 轮播 -->
    <view class="page-section page-section-spacing swiper">
      <swiper
        indicator-dots
        autoplay
        circular
        indicator-active-color="#fff"
        interval="3000"
        duration="1000"
      >
        <block v-for="item in banner" :key="item.id">
          <swiper-item>
            <image :src="item.thumb" />
          </swiper-item>
        </block>
      </swiper>
    </view>

    <!-- 列表 -->
    <view class="album_list">
      <view  class="album_item" v-for="item in albumList " :key="item.id">
        <navigator :url="`/pages/album/index?id=${item.id}`" class="album_img">
          <image :src="item.cover" mode="aspectFill" />
        </navigator>
        <view class="album_info">
          <view class="album_name">{{item.name}}</view>
          <view class="album_desc">{{item.desc}}</view>
          <view class="album_btn">
            <view class="album_attention">关注</view>
          </view>
        </view>
      </view>
    </view>
  </scroll-view>
</template>

<script>
import moment from "moment";
export default {
  data() {
    return {
      params: {
        limit: 30,
        skip: 0,
        order: "new"
      },
      banner: [], // 轮播图
      albumList: [], //列表
      hasMore: true
    };
  },
  mounted() {
    this.getData();
    uni.setNavigationBarTitle({
      title: "专辑"
    });
  },
  methods: {
    handleToLower() {
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
    getData() {
      this.request({
        url: "http://157.122.54.189:9088/image/v1/wallpaper/album",
        data: this.params
      }).then(result => {
        console.log(result);
        if (this.banner.length == 0) {
          this.banner = result.res.banner;
        }
        // 判断有没有下一页数据
        if (result.res.album.length == 0) {
          this.hasMore = false;
          uni.showToast({
            title: "没有更多数据了",
            icon: "none"
          });
          return;
        }
        this.albumList = [...this.albumList, ...result.res.album];
      });
    }
  }
};
</script>

<style lang="scss" scoped>
.album_scroll_view {
  height: calc(100vh - 36px);
}
.swiper {
  swiper {
    height: 320rpx;
    image {
      height: 100%;
    }
  }
}

.album_list {
  padding: 10rpx;
  .album_item {
    padding: 10rpx 0;
    display: flex;
    border-bottom: 1rpx solid #ccc;
    .album_img {
      flex: 1;
      padding: 10rpx;
      image {
        width: 200rpx;
        height: 200rpx;
      }
    }

    .album_info {
      flex: 2;
      padding: 0 10rpx;
      overflow: hidden;
      .album_name {
        font-size: 30rpx;
        color: #000;
        padding: 10rpx 0;
      }

      .album_desc {
        padding: 10rpx 0;
        font-size: 24rpx;

        text-overflow: ellipsis;
        overflow: hidden;
        white-space: nowrap;
      }

      .album_btn {
        padding: 10rpx;
        display: flex;
        justify-content: flex-end;
        .album_attention {
          color: #d83a88;
          border: 1rpx solid #d83a88;
          padding: 10rpx;
        }
      }
    }
  }
}
</style>