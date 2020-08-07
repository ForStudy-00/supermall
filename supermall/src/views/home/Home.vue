<template>
  <div id="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>
    <tab-control
      :titles="['流行','新款','精选']"
      @tabClick="tabClick"
      ref="tabControl1"
      v-show="isTabShow"
      class="tabcontrol"
    ></tab-control>
    <scroll
      class="content"
      ref="scroll"
      :probe-type="3"
      @scroll="contentScroll"
      :pull-up-load="true"
      @pullingUp="loadMore"
    >
      <home-swiper :banners="banners" @swipperImageLoad="swipperImageLoad" />
      <recommend-view :recommends="recommends" />
      <feature-view></feature-view>
      <tab-control :titles="['流行','新款','精选']" @tabClick="tabClick" ref="tabControl2"></tab-control>
      <goods-list :goods="showGoods"></goods-list>
    </scroll>
    <back-top @click.native="backTopClick" v-show="isShowBackTop"></back-top>
  </div>
</template>

<script>
import HomeSwiper from './childComps/HomeSwiper'
import RecommendView from './childComps/RecommendView'
import FeatureView from './childComps/FeatureView'
import GoodsList from '../../components/content/goods/GoodsList'
import GoodsListItem from '../../components/content/goods/GoodsListItem'

import NavBar from '../../components/common/navbar/NavBar'
import TabControl from '../../components/content/tabControl/TabControl'
import BackTop from '../../components/content/backTop/BackTop'

import { getHomeMultidata, getHomeGoods } from "../../network/home"
import Scroll from '../../components/common/scroll/Scroll'
import { debounce } from '../../common/utils/debounce'

export default {
  name: "Home",
  components: {
    HomeSwiper,
    FeatureView,
    RecommendView,
    GoodsList,
    GoodsListItem,
    NavBar,
    TabControl,
    Scroll,
    BackTop
  },
  data () {
    return {
      banners: [],
      recommends: [],
      goods: {
        'pop': { page: 0, list: [] },
        'new': { page: 0, list: [] },
        'sell': { page: 0, list: [] },
      },
      currentType: 'pop',
      isShowBackTop: false,
      isTabShow: false,
      offsetTop: 0,
      saveY: 0
    }
  },
  computed: {
    showGoods () {
      return this.goods[this.currentType].list
    },
  },
  created () {
    // 1.请求多个数据

    this.getHomeMultidata()

    //2.请求商品数据
    this.getHomeGoods('pop')
    this.getHomeGoods('new')
    this.getHomeGoods('sell')

  },
  activated () {
    this.$refs.scroll.refresh()
    this.$refs.scroll.scrollTo(0, this.saveY, 1)
  },
  deactivated () {
    this.saveY = this.$refs.scroll.getScrollY()
  },
  mounted () {
    const refresh = debounce(this.$refs.scroll.refresh, 200)
    this.$bus.$on('itemImageLoad', () => {
      refresh()
    })
  },
  methods: {

    /**
     * 事件监听相关代码
     */
    tabClick (index) {
      this.currentType = Object.keys(this.goods)[index]
      this.$refs.tabControl1.currentIndex = index
      this.$refs.tabControl2.currentIndex = index
    },
    backTopClick () {
      // console.log(this.$refs.scroll);
      this.$refs.scroll.scrollTo(0, 0, 500)
    },
    contentScroll (position) {
      this.isShowBackTop = position.y < -1000
      // console.log(this.offsetTop);
      // console.log(position.y < -this.offsetTop);
      this.isTabShow = position.y - 44 < -this.offsetTop
    },
    loadMore () {
      this.getHomeGoods(this.currentType)
    },
    swipperImageLoad () {
      this.offsetTop = this.$refs.tabControl2.$el.offsetTop
    },
    /**
     * 网络请求相关代码
     */
    getHomeMultidata () {
      getHomeMultidata().then(res => {
        // this.result = res;
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
      })
    },
    getHomeGoods (type) {
      const page = this.goods[type].page + 1
      getHomeGoods(type, page).then(res => {
        this.goods[type].list.push(...res.data.list)
        this.goods[type].page += 1
        this.$refs.scroll.finishPullUp()
      })
    }
  },
}
</script>

<style scoped>
#home {
  padding-top: 44px;
  position: relative;
}
.home-nav {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  background-color: var(--color-tint);
  color: #fff;
  z-index: 9;
}
.content {
  height: calc(100vh - 93px);
}
.tabcontrol {
  width: 100%;
  position: fixed;
  display: flex;
  z-index: 3;
}
.tabcontrol span {
  flex: 1;
}
</style>
