<template>
  <view @touchstart="handleTouchStart" @touchend="handleTouchEnd">
    <slot></slot>
  </view>
</template>

<script>
export default {
  data() {
    return {
      startTime: 0,
      startX: 0,
      startY: 0,
      endTime: 0,
      endX: 0,
      endY: 0
    };
  },
  methods: {
    handleTouchStart(event) {
      this.startTime = Date.now();
      this.startX = event.changedTouches[0].clientX;
      this.startY = event.changedTouches[0].clientY;
    },
    handleTouchEnd(event) {
      this.endTime = Date.now();
      this.endX = event.changedTouches[0].clientX;
      this.endY = event.changedTouches[0].clientY;
      let direction = "";
      if (this.endTime - this.startTime > 2000) {
        return;
      }

      if (Math.abs(this.endX - this.startX) > 10) {
        this.endX - this.startX > 0
          ? (direction = "right")
          : (direction = "left");
      } else {
        return;
      }
      this.$emit("swiperAction", { direction });
    }
  }
};
</script>
</script>

<style>
</style>