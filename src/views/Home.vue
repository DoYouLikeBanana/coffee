<template>
  <div class="home">
    <van-nav-bar>
      <template #left>
        <div class="home-nav">
          <div class="t1">{{ hoursTip }}</div>
          <div class="t2">{{ userInfo.nickName }}</div>
        </div>
      </template>
      <template #right>
        <div class="home-search">
          <van-search placeholder="输入商品名称" @focus="searchFocus" />
        </div>
      </template>
    </van-nav-bar>

    <div class="home-content">
      <!-- 轮播图 -->
      <div class="banner-box">
        <van-swipe @change="changeCurrentIndex" :autoplay="5000" loop>
          <van-swipe-item v-for="(item, index) in bannerData" :key="index">
            <img
              class="auto-img"
              :src="item.bannerImg"
              alt=""
              @click="goDetail(item.pid)"
            />
          </van-swipe-item>
          <template #indicator>
            <div class="index-box">
              <div
                :class="{ active: index == currentIndex }"
                v-for="(item, index) in bannerData"
                :key="index"
              ></div>
            </div>
          </template>
        </van-swipe>
      </div>

      <!-- 商品 -->
      <div class="product-box">
        <div>
          <div class="clearfix pro-title-box">
            <span class="pro-title">热卖推荐</span>
          </div>
          <div class="products clearfix">
            <div
              class="pro-item fl"
              v-for="(item, index) in hotProduct"
              :key="index"
              @click="goDetail(item.pid)"
            >
              <div>
                <img class="auto-img" :src="item.smallImg" />
              </div>
              <div class="pro-info">
                <div class="pro-name one-text">{{ item.name }}</div>
                <div class="pro-enname one-text">{{ item.enname }}</div>
                <div class="pro-price">￥{{ item.price }}</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import "../assets/scss/home.scss";
export default {
  name: "Home",
  data() {
    return {
      //当前轮播图片索引
      currentIndex: 0,

      //轮播数据
      bannerData: [],

      //热卖商品数据
      hotProduct: [],

      //用户信息
      userInfo: {},

      //时间提示
      hoursTip: "",
    };
  },

  created() {
    //获取轮播图数据
    this.getBannerData();

    //获取热卖推荐商品
    this.getHotProduct();

    //查询用户信息
    this.getUserInfo();

    //时间提示
    this.getTime();
  },

  methods: {
    //修改轮播图片索引
    changeCurrentIndex(index) {
      this.currentIndex = index;
    },

    //时间提示
    getTime() {
      let self = this;

      let date = new Date();

      if (date.getHours() >= 0 && date.getHours() < 12) {
        self.hoursTip = "上午好";
      } else if (date.getHours() >= 12 && date.getHours() < 18) {
        self.hoursTip = "下午好";
      } else {
        self.hoursTip = "晚上好";
      }
    },

    //获取轮播图数据
    getBannerData() {
      this.$toast.loading({
        message: "加载中...",
        forbidClick: true,
        duration: 0,
      });

      //发起注册请求
      this.axios({
        //请求类型
        method: "GET",
        //请求路径
        url: "/banner",

        //GET请求参数, object
        params: {
          appkey: this.appkey,
        },
      })
        .then((result) => {
          this.$toast.clear();

          if (result.data.code == 300) {
            this.bannerData = result.data.result;
          }
        })
        .catch((err) => {
          this.$toast.clear();
        });
    },

    //获取热卖推荐商品
    getHotProduct() {
      this.$toast.loading({
        message: "加载中...",
        forbidClick: true,
        duration: 0,
      });

      //发起注册请求
      this.axios({
        //请求类型
        method: "GET",
        //请求路径
        url: "/typeProducts",

        //GET请求参数, object
        params: {
          appkey: this.appkey,
          key: "isHot",
          value: 1,
        },
      })
        .then((result) => {
          this.$toast.clear();

          if (result.data.code == 500) {
            this.hotProduct = result.data.result;
          }
        })
        .catch((err) => {
          this.$toast.clear();
        });
    },

    //查看商品详情页面
    goDetail(pid) {
      this.$router.push({ name: "Detail", params: { pid } });
    },

    //获取用户信息
    getUserInfo() {
      let tokenString = localStorage.getItem("__tk");

      if (!tokenString) {
        return;
      }

      this.$toast.loading({
        message: "加载中...",
        forbidClick: true,
        duration: 0,
      });

      this.axios({
        method: "GET",
        url: "/findMy",
        params: {
          appkey: this.appkey,
          tokenString,
        },
      })
        .then((result) => {
          this.$toast.clear();

          if (result.data.code == "A001") {
            this.userInfo = result.data.result[0];
          }
        })
        .catch((err) => {
          this.$toast.clear();
        });
    },

    //搜索栏获取焦点
    searchFocus() {
      this.$router.push({ name: "Search" });
    },
  },
};
</script>