<template>
  <view>
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
            <text>18</text>01月
          </view>
        </view>
        <view class="monthes_text">你负责美丽</view>
      </view>
      <view class="monthes_title_more">更多</view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      params: {
        limit: 30,
        skip: 0,
        order: "hot"
      },
      // 推荐列表
      recommendList: []
    };
  },
  mounted() {
    this.request({
      url: "http://157.122.54.189:9088/image/v3/homepage/vertical",
      data: this.params
    }).then(res => {
      console.log(res);
      this.recommendList = res.res.homepage[1].items;
    });
  }
};
</script>

<style lang="scss">
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
    .monthes_title_info {
      .monthes_info {
        text {

        }
      }
    }

    .monthes_text {

    }
  }

  .monthes_title_more {

  }
}
</style>