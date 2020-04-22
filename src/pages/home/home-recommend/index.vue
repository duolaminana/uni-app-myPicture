<template>
  <scroll-view
    scroll-y
    class="recommend_view"
    @scrolltolower="handleToLower"
    v-if="recommendList.length>0"
  >
    <!-- 推荐 -->
    <view class="recommend_wrap">
      <view class="recommend_item" v-for="item in recommendList" :key="item.id">
        <image mode="widthFix" :src="item.thumb" />
      </view>
    </view>

    <!-- 月份 -->
    <view class="monthes_wrap">
      <view class="monthes_title">
        <view class="monthes_title_info">
          <view class="monthes_info">
            <text>{{monthes.DD}}/</text>
            {{monthes.MM}}月
          </view>
          <view class="monthes_text">{{monthes.title}}</view>
        </view>
        <view class="monthes_title_more">更多></view>
      </view>
      <view class="monthes_content">
        <view class="monthes_item" v-for="item in monthes.items" :key="item.id">
          <image :src="item.thumb+item.rule.replace('$<Height>',360)" mode="scaleToFill" />
        </view>
      </view>
    </view>

    <!-- 热门 -->
    <view class="hots_wrap">
      <view class="hots_title">
        <text>热门</text>
      </view>
      <view class="hots_content">
        <view class="hot_item" v-for="item in hots" :key="item.id">
          <image :src="item.thumb" mode="scaleToFill" />
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
        order: "hot"
      },
      recommendList: [], // 推荐列表
      monthes: {}, //月份
      hots: [], //热门
      hasMore: true
    };
  },
  mounted() {
    this.getData();
    uni.setNavigationBarTitle({
      title:"推荐"
    })
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
        url: "http://157.122.54.189:9088/image/v3/homepage/vertical",
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

        if (this.hots.length == 0) {
          this.recommendList = result.res.homepage[1].items;
          this.monthes = result.res.homepage[2];
          this.monthes.MM = moment(this.monthes.stime).format("MM");
          this.monthes.DD = moment(this.monthes.stime).format("DD");
        }
        this.hots = [...this.hots, ...result.res.vertical];
      });
    }
  }
};
</script>

<style lang="scss" scoped>
.recommend_view {
  height: calc(100vh - 36px);
}
.recommend_wrap {
  display: flex;
  flex-wrap: wrap;
  .recommend_item {
    width: 50%;
    border: 3rpx solid #fff;
  }
}

.monthes_wrap {
  .monthes_title {
    display: flex;
    justify-content: space-between;
    padding: 20rpx;

    .monthes_title_info {
      color: #d83a88;
      font-size: 30rpx;
      font-weight: 600;
      display: flex;
      .monthes_info {
        text {
          font-size: 36rpx;
        }
      }
    }

    .monthes_text {
      font-size: 34rpx;
      color: #666;
      margin-left: 30rpx;
    }
    .monthes_title_more {
      font-size: 24rpx;
      color: #d83a88;
    }
  }

  .monthes_content {
    display: flex;
    flex-wrap: wrap;
    .monthes_item {
      width: 33.33%;
      border: 5rpx solid #fff;
    }
  }
}

.hots_wrap {
  .hots_title {
    padding: 20rpx;
    text {
      border-left: 20rpx solid #d83a88;
      padding-left: 20rpx;
      font-size: 34rpx;
      font-weight: 600;
    }
  }

  .hots_content {
    display: flex;
    flex-wrap: wrap;

    .hot_item {
      width: 33.33%;
      border: 5rpx solid #fff;
      image {
      }
    }
  }
}
</style>