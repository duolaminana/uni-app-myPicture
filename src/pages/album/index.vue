<template>
  <view>
    <!-- 专辑背景 -->
    <view class="album_bg">
      <image :src="album.cover" mode="widthFix" />
      <view class="album_info">
        <view class="ablum_name">{{album.name}}</view>
        <view class="ablum_btn">关注专辑</view>
      </view>
    </view>
    <!-- 专辑作者 -->
    <view class="album_author">
      <view class="album_author_info">
        <image :src="album.user.avatar" mode="widthFix" />
        <view class="author_name">{{album.user.name}}</view>
      </view>
      <view class="album_author_desc">{{album.desc}}</view>
    </view>

    <!-- 列表 -->
    <view class="album_list">
      <view class="album_item" v-for="(item,index) in albumList" :key="item.id">
        <go-detail :list="albumList" :index="index">
        <image :src="item.img" mode="aspectFill" />
        </go-detail>
      </view>
    </view>
  </view>
</template>

<script>
import moment from "moment";
import goDetail from "@/components/goDetail";
export default {
  data() {
    return {
      params: {
        limit: 30,
        skip: 0,
        order: "new",
        first: 1
      },
      albumList: [], //列表
      hasMore: true,
      id: null,
      album: {},
      wallpaper: []
    };
  },
  components: {
    goDetail
  },
  onLoad(e) {
    this.id="5d5f8e45e7bce75ae7fb8278";
    // this.id = e.id;
    console.log(this.id);
    this.getData();
  },
  // 上拉触底事件
    onReachBottom() {
      if (this.hasMore) {
        this.first = 0;
        this.params.skip += this.params.limit;
        this.getData();
      } else {
        uni.showToast({
          title: "没有更多数据了",
          icon: "none"
        });
      }
    },
  methods: {
    
    getData() {
      this.request({
        url: `http://157.122.54.189:9088/image/v1/wallpaper/album/${this.id}/wallpaper`,
        data: this.params
      }).then(result => {
        if (Object.keys(this.album).length === 0) {
          this.album = result.res.album;
        }
        // 判断有没有下一页数据
        if (result.res.wallpaper.length == 0) {
          this.hasMore = false;
          uni.showToast({
            title: "没有更多数据了",
            icon: "none"
          });
          return;
        }
        this.albumList = [...this.albumList, ...result.res.wallpaper];
      });
    }
  }
};
</script>

<style lang="scss" scoped>
.album_bg {
  position: relative;
  // image {
  // }
  .album_info {
    position: absolute;
    width: 100%;
    left: 0;
    bottom: 0;
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 80rpx;
    padding: 0 20rpx;
    color: #fff;
    .ablum_name {
      font-size: 40rpx;
    }

    .ablum_btn {
      width: 152rpx;
      line-height: 60rpx;
      text-align: center;
      border-radius: 10rpx;
      background-color: $color;
      color: #fff;
    }
  }
}

.album_author {
  padding: 10rpx;
  .album_author_info {
    padding: 10rpx 0;
    display: flex;
    image {
      width: 50rpx;
    }

    .author_name {
      color: #000;
      margin-left: 15rpx;
    }
  }

  // .album_author_desc {
  // }
}
.album_list {
  display: flex;
  flex-wrap: wrap;
  .album_item {
    width: 33.33%;
    border: 5rpx solid #fff;
    image {
      height: 160rpx;
    }
  }
}
</style>