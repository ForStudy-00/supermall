<template>
  <div id="detail">
    <detail-nav-bar class="nav-bar" @titleClick="titleClick" />
    <scroll class="content" ref="scroll">
      <detail-swiper :top-images="topImages" />
      <detail-base-info :goods="goods" />
      <detail-shop-info :shop="shop" />
      <detail-goods-info :detailInfo="detailInfo" @imageLoad="imageLoad" />
      <detail-param-info :paramInfo="paramInfo" />
      <detail-comment-info :commentInfo="commentInfo" />
      <goods-list :recommend="recommends" />
    </scroll>
  </div>
</template>
<script>
import DetailNavBar from './childComps/DetailNavBar'
import DetailSwiper from './childComps/DetailSwiper'
import DetailBaseInfo from './childComps/DetailBaseInfo'
import DetailShopInfo from './childComps/DetailShopInfo'
import DetailGoodsInfo from './childComps/DetailGoodsInfo'
import DetailParamInfo from './childComps/DetailParamInfo'
import DetailCommentInfo from './childComps/DetailCommentInfo'
import { getDetails, Goods, Shop, GoodsParam, getRecommend } from '../../network/detail'
import Scroll from '../../components/common/scroll/Scroll'
import GoodsList from '../../components/content/goods/GoodsList'
import { debounce } from '../../common/utils/debounce'
export default {
  name: "Detail",
  components: {
    DetailNavBar,
    DetailSwiper,
    DetailBaseInfo,
    DetailShopInfo,
    DetailGoodsInfo,
    DetailParamInfo,
    DetailCommentInfo,
    GoodsList,
    Scroll
  },
  data () {
    return {
      iid: null,
      topImages: [],
      goods: {},
      shop: {},
      detailInfo: {},
      paramInfo: {},
      commentInfo: {},
      recommends: {},
      themeTopYs: []
    }
  },
  created () {
    this.iid = this.$route.params.iid,
      getDetails(this.iid).then(res => {
        const data = res.result
        this.topImages = data.itemInfo.topImages
        this.goods = new Goods(data.itemInfo, data.columns, data.shopInfo.services)
        this.shop = new Shop(data.shopInfo)
        this.detailInfo = data.detailInfo
        this.paramInfo = new GoodsParam(data.itemParams.info, data.itemParams.rule)
        if (data.rate.cRate != 0) {
          this.commentInfo = data.rate.list[0]
        }
        getRecommend().then(res => {
          this.recommends = res.data.list
        })
      })
  },
  mounted () {
    const refresh = debounce(this.$refs.scroll.refresh, 200)
    this.$bus.$on('detailImageLoad', () => {
      refresh()
    })
  },
  methods: {
    imageLoad () {
      this.$refs.scroll.refresh()
    },
    titleClick (index) {
      console.log(index);
    }
  },
}
</script>
<style scoped>
#detail {
  position: relative;
  z-index: 5;
  background-color: #fff;
}
.content {
  height: calc(100vh - 44px);
}
.nav-bar {
  position: relative;
  z-index: 9;
  background-color: #fff;
}
</style>