<template>
  <div id="home">
    <nav-bar class="nav-bar">
      <div slot="center">购物街</div>
    </nav-bar>
    <div class="scrool">
      <home-swiper :banners="banners"></home-swiper>
      <recommend :recommends="recommends"></recommend>
      <future></future>
      <tab-control
        :title="['流行', '新款', '精选']"
        @itemClick="tabClick"
      ></tab-control>
      <goods-list :goods="showGoods"></goods-list>
    </div>
    <ul>
      <li>列表1</li>
      <li>列表2</li>
      <li>列表3</li>
      <li>列表4</li>
      <li>列表5</li>
      <li>列表6</li>
      <li>列表7</li>
      <li>列表8</li>
    </ul>
  </div>
</template>

<script>
import HomeSwiper from "./childComps/HomeSwiper";
import Recommend from "./childComps/Recommend.vue";
import Future from "./childComps/Future.vue";

import NavBar from "components/common/navbar/NavBar";
import TabControl from "components/content/tabControl/TabControl.vue";
import GoodsList from "components/content/goods/GoodsList.vue";

import { getHomeMultidata, getHomeGoods } from "network/home.js";

import { debounce, debounce1 } from "common/utils";

export default {
  name: "Home",
  components: {
    HomeSwiper,
    Recommend,
    Future,
    NavBar,
    TabControl,
    GoodsList,
  },
  data() {
    return {
      banners: [],
      recommends: [],
      goods: {
        pop: { page: 0, list: [] },
        new: { page: 0, list: [] },
        sell: { page: 0, list: [] },
      },
      currentType: "pop",
    };
  },
  created() {
    // 请求多个数据
    this.getHomeMultidata();
    // 请求商品数据
    this.getHomeGoods("pop");
    this.getHomeGoods("new");
    this.getHomeGoods("sell");
    //监听事件总线传递过来的事件
    const refresh = debounce1(this.testDebounce);
    this.$bus.$on("imageLoad", () => {
      refresh("shiny", "love", "jamie");
      // this.testDebounce();
    });
  },
  computed: {
    showGoods() {
      return this.goods[this.currentType].list;
    },
  },
  methods: {
    testDebounce(...arg) {
      // console.log("---------");
      console.log(arg);
    },
    getHomeMultidata() {
      getHomeMultidata().then((res) => {
        // console.log(res);
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
      });
    },
    getHomeGoods(type) {
      const page = this.goods[type].page + 1;
      getHomeGoods(type, page).then((res) => {
        console.log(res);
        this.goods[type].list.push(...res.data.list);
        this.goods[type].page = page + 1;
      });
    },
    // 通过tabcontrol，监听他的点击事件，将点击索引值通过$emit传递给父组件，将父组件的数据与索引值对应，记录当前的数据类型
    // 再将当前数据类型，展示给子组件
    tabClick(index) {
      switch (index) {
        case 0:
          this.currentType = "pop";
          break;
        case 1:
          this.currentType = "new";
          break;
        case 2:
          this.currentType = "sell";
          break;
      }
    },
  },
};
</script>

<style scoped>
#home {
  position: relative;
}
.nav-bar {
  position: fixed;
  left: 0;
  right: 0;
  top: 0;
  z-index: 9;
  background-color: #f19ea2;
  color: #fff;
}
.scroll {
  position: absolute;
  top: 44px;
}
</style>