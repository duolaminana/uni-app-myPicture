<template>
  <view class="video_play">
    <image :src="videoObj.img" mode="scaleToFill" />
    <!-- 工具栏 -->
    <view class="video_tool">
      <view :class="['iconfont',muted?'iconjingyin':'iconshengyin']" @click="muted=!muted"></view>
      <view class="iconfont iconzhuanfa">
        <button open-type="share"></button>
      </view>
    </view>

    <!-- 视频播放 -->
    <view class="video_wrap">
      <video :src="videoObj.video" :muted="muted" objectFit="fill"></video>
    </view>
    <!-- 下载 按钮 -->
    <view class="download">
      <view class="download_btn" @click="handleDownload">下载高清</view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      videoObj: {},
      muted: false
    };
  },
  onLoad() {
    console.log(getApp().globalData.video);
    this.videoObj = getApp().globalData.video;
  },
  methods: {
    async handleDownload() {
      await uni.showLoading({ title: "下载中" });
      // const { tempFilePath } = await uni.downloadFile({ url: this.videoObj.video })[1]
      const result1 = await uni.downloadFile({ url: this.videoObj.video });
      const{tempFilePath}=result1[1]

      await uni.saveImageToPhotosAlbum({
        filePath:tempFilePath
      })
      uni.hideLoading()
      await uni.showToast({title:"下载成功"})
    }
  }
};
</script>

<style lang="scss" scoped>
.video_play {
  position: relative;
  image {
    position: absolute;
    width: 100vw;
    height: 100vh;
    z-index: -1;
    filter: blur(20px);
  }

  .video_tool {
    height: 80rpx;
    display: flex;
    justify-content: flex-end;

    .iconfont {
      width: 80rpx;
      color: #fff;
      font-size: 50rpx;
      border-radius: 50%;
      background-color: rgba(0, 0, 0, 0.3);
      display: flex;
      justify-content: center;
      align-items: center;
      margin-right: 20rpx;
    }
    .iconzhuanfa {
      position: relative;
      button {
        position: absolute;
        width: 100%;
        height: 100%;
        opacity: 0;
      }
    }
  }

  .video_wrap {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    video {
      width: 360rpx;
      height: 600rpx;
    }
  }

  .download {
    display: flex;
    justify-content: center;
    margin-top: 30rpx;
    .download_btn {
      width: 360rpx;
      height: 80rpx;
      border-radius: 40rpx;
      display: flex;
      justify-content: center;
      align-items: center;
      color: #fff;
      border: 1rpx solid #fff;
      background-color: rgba(0, 0, 0, 0.6);
    }
  }
}
</style>