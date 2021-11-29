<template>
  <div id="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>
    <scroll class="content" ref="scroll" @scroll="contentScroll" @pullingUp="loadMore">
      <home-swiper :banners="banners"></home-swiper>
      <recommend-view :recommends="recommends"></recommend-view>
      <feature-view/>
      <tab-control :titles="['流行','新款','精选']" @tabClick="tabClick"></tab-control>
      <goods-list :goods="showGoods"/>
    </scroll>
    <BackTop @click.native="backtop" v-show="isShowBack"/>
  </div>
</template>

<script>
import NavBar from "components/common/navbar/NavBar";
import HomeSwiper from "./childComps/HomeSwiper";
import RecommendView from "./childComps/RecommendView";
import FeatureView from "./childComps/FeatureView";
import GoodsList from "components/content/goods/GoodsList";


import {getHomeMultidata, getHomeGoods} from "network/home";
import TabControl from "components/content/tabControl/TabControl";
import scroll from "components/common/scroll";
import BackTop from "components/content/backTop/BackTop";


export default {
  name: "Home",
  components: {
    TabControl,
    NavBar,
    HomeSwiper,
    RecommendView,
    FeatureView,
    GoodsList,
    scroll,
    BackTop
  },
  data() {
    return {
      banners: [],
      recommends: [],
      goods: {
        'pop': {page: 0, list: []},
        'new': {page: 0, list: []},
        'sell': {page: 0, list: []},
      },
      currentType: 'pop',
      isShowBack: false
    }
  },
  computed: {
    showGoods() {
      return this.goods[this.currentType].list
    }
  },
  created() {
    this.getHomeMultidata()
    //请求商品数据
    this.getHomeGoods('pop')
    this.getHomeGoods('new')
    this.getHomeGoods('sell')
  },
  mounted() {

  },
  methods: {
    tabClick(index) {
      switch (index) {
        case 0:
          this.currentType = 'pop'
          break
        case 1:
          this.currentType = 'new'
          break
        case 2:
          this.currentType = 'sell'
          break
      }
    }
    ,
    getHomeMultidata() {
      getHomeMultidata().then(res => {
        this.banners = res.data.data.banner.list;
        this.recommends = res.data.data.recommend.list;
      })

    }
    ,
    getHomeGoods(type) {
      const page = this.goods[type].page + 1
      getHomeGoods(type, page).then(res => {
        this.goods[type].list.push(...res.data.data.list)
        this.goods[type].page += 1

        // 完成上拉加载更多
        this.$refs.scroll.finishPullUp()
      })
    }
    ,
    backtop() {
      this.$refs.scroll.scrollTo(0, 0)
    },
    contentScroll(postion) {
      this.isShowBack = -postion.y > 1000

    },
    loadMore(){
      console.log('shangla')
      this.getHomeGoods(this.currentType)
    }
  }
}


</script>

<style scoped>
#home {
  height: 100vh;
}

.home-nav {
  background-color: palevioletred;
  color: #fff;
}

.content {
  overflow: hidden;
  height: calc(100% - 60px);
}
</style>
