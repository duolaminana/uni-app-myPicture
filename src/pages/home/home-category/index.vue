<template>
  <view class="category">
    <view class="cate_item" v-for="item in category" :key="item.id">
      <navigator :url="`/pages/imgCategroy/index?id=${item.id}`">
        <image :src="item.cover" mode="aspectFill" />
      </navigator>
      <view class="cate_name">{{item.name}}</view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      category: []
    };
  },
  mounted() {
    uni.setNavigationBarTitle({
      title: "分类"
    });
    this.getData();
  },
  methods: {
    getData() {
      this.request({
        url: "http://157.122.54.189:9088/image/v1/vertical/category"
      }).then(result => {
        console.log(result);
        this.category = result.res.category;
      });
    }
  }
};
</script>

<style lang="scss" scoped>
.category {
  display: flex;
  flex-wrap: wrap;
  .cate_item {
    width: 33.33%;
    position: relative;
    border: 5rpx solid #fff;
    image {
      height: 240rpx;
    }
    .cate_name {
      width: 100%;
      position: absolute;
      left: 0;
      bottom: 0;
      font-size: 40rpx;
      color: #fff;
      background-image: linear-gradient(
        to right top,
        rgba(0, 0, 0, 0.2),
        rgba(0, 0, 0, 0)
      );
      display: flex;
      padding-left: 20rpx;

    }
  }
}
</style>