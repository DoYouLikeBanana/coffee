<!-- 进入时广告 -->
<template>
  <div class="box">
    <van-swipe
      class="my-swipe"
      :loop="false"
      indicator-color="#55aaff"
      @change="onChange"
    >
      <van-swipe-item v-for="(item, index) in imgurl" :key="index"
        ><img :src="item.url" alt=""
      /></van-swipe-item>
    </van-swipe>
  </div>
</template>

<script>
export default {
  data() {
    return {
      imgurl: [
        { url: require("../assets/images/timg1.jpg") },
        { url: require("../assets/images/time2.jpg") },
        { url: require("../assets/images/time3.jpg") },
        { url: require("../assets/images/time4.jpg") },
      ],
    };
  },
  methods: {
    onChange(index) {
      if (index == 3) {
        setTimeout(() => {
          this.$router.push({
            name: "Home",
          });
        }, 500);
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.box {
  width: 100%;
  height: 100%;
  .van-swipe {
    height: 100%;
  }
}
.my-swipe .van-swipe-item {
  color: #fff;
  font-size: 20px;
  /* line-height: 150px; */
  text-align: center;
  background-color: #fff;
  height: 100%;
  /* height: 900px; */
  img {
    width: 100%;
    height: 100%;
  }
}
</style>
