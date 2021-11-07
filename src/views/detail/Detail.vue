<template>
  <div id="detail">
    <DetailNavBar
      class="detail-nav"
      @titleClick="titleClick"
      ref="nav"
    ></DetailNavBar>
    <Scroll class="content" ref="scroll" :probeType="3" @scroll="contentScroll">
      <DetailSwiper :topImages="topImages"></DetailSwiper>
      <DetailBaseInfo :goods="goods"></DetailBaseInfo>
      <DetailShopInfo :shop="shop"></DetailShopInfo>
      <DetailGoodsInfo
        :detailInfo="detailInfo"
        @imageLoad="imageLoad"
      ></DetailGoodsInfo>
      <DetailParamInfo :paramInfo="paramInfo" ref="params"></DetailParamInfo>
      <DetailCommentInfo
        :commentInfo="commentInfo"
        ref="comment"
      ></DetailCommentInfo>
      <GoodsList :goods="recommends" ref="recommend"></GoodsList>
    </Scroll>
    <BackTop @click.native="backClick" v-show="isShowBackTop"></BackTop>
    <DetailBottomBar @addToCart="addToCart"></DetailBottomBar>
  </div>
</template>

<script>
import DetailNavBar from './childComponents/DetailNavBar.vue'
import DetailSwiper from './childComponents/DetailSwiper.vue'
import DetailBaseInfo from './childComponents/DetailBaseInfo.vue'
import DetailShopInfo from './childComponents/DetailShopInfo.vue'
import DetailGoodsInfo from './childComponents/DetailGoodsInfo.vue'
import DetailParamInfo from './childComponents/DetailParamInfo.vue'
import DetailCommentInfo from './childComponents/DetailCommentInfo.vue'
import DetailBottomBar from './childComponents/DetailBottomBar.vue'
import GoodsList from '../../components/content/goods/GoodsList.vue'
import {
  getDetail,
  getRecommend,
  Goods,
  Shop,
  GoodsParam
} from '../../network/detail'

import Scroll from '../../components/common/scroll/Scroll.vue'

import { itemListenerMixin, backTopMixin } from '../../common/mixin'
import { debounce } from '../../common/utils'
export default {
  name: 'Detail',
  mixins: [itemListenerMixin, backTopMixin],
  data() {
    return {
      iid: null,
      topImages: [],
      goods: {},
      shop: {},
      detailInfo: {},
      paramInfo: {},
      commentInfo: {},
      recommends: [],
      themeTopYs: [],
      getThemeTopY: null,
      currentIndex: null
    }
  },
  components: {
    DetailNavBar,
    DetailSwiper,
    DetailBaseInfo,
    DetailShopInfo,
    DetailGoodsInfo,
    DetailParamInfo,
    DetailCommentInfo,
    GoodsList,
    DetailBottomBar,
    Scroll
  },
  created() {
    this.iid = this.$route.params.iid
    getDetail(this.iid).then((res) => {
      const data = res.result
      // console.log(data)
      this.topImages = data.itemInfo.topImages
      // this.goods = new Goods(data.itemInfo, data.columns, data.shopInfo)
      this.goods = new Goods(
        data.itemInfo,
        data.columns,
        data.shopInfo.services
      )
      // console.log(this.goods)
      this.shop = new Shop(data.shopInfo)
      // console.log(data.shopInfo)
      this.detailInfo = data.detailInfo

      this.paramInfo = new GoodsParam(
        data.itemParams.info,
        data.itemParams.rule
      )

      if (data.rate.cRate !== 0) {
        this.commentInfo = data.rate.list[0]
      }
      // console.log(this.commentInfo)
      // this.$nextTick(() => {})
    })

    this.getThemeTopY = debounce(() => {
      this.themeTopYs = []
      this.themeTopYs.push(0)
      this.themeTopYs.push(this.$refs.params.$el.offsetTop)
      this.themeTopYs.push(this.$refs.comment.$el.offsetTop)
      this.themeTopYs.push(this.$refs.recommend.$el.offsetTop)
      // console.log(this.themeTopYs)
    }, 500)

    getRecommend().then((res) => (this.recommends = res.data.list))
  },
  mounted() {},
  updated() {},
  methods: {
    imageLoad() {
      this.$refs.scroll.refresh()

      this.getThemeTopY()
    },
    titleClick(index) {
      // console.log('click')

      this.$refs.scroll.scrollTo(0, -this.themeTopYs[index], 100)
    },
    contentScroll(position) {
      // console.log(position)
      const positionY = -position.y
      const length = this.themeTopYs.length
      for (let i = 0; i < length; i++) {
        // console.log(i)
        if (
          this.currentIndex !== i &&
          ((i < length - 1 &&
            positionY >= this.themeTopYs[i] &&
            positionY < this.themeTopYs[i + 1]) ||
            (i === length - 1 && positionY >= this.themeTopYs[i]))
        ) {
          // console.log(i)
          this.currentIndex = i
          this.$refs.nav.currentIndex = this.currentIndex
        }
      }
      this.listenShowBackTop(position)
    },
    addToCart() {
      // console.log('outer-add')
      const product = {}
      product.image = this.topImages[0]
      product.title = this.goods.title
      product.desc = this.goods.desc
      product.price = this.goods.realPrice
      product.iid = this.iid
      this.$store.dispatch('addCart', product)
    }
  },
  destroyed() {
    this.$bus.$off('itemImgLoad', this.itemImgListener)
  }
}
</script>

<style scoped>
#detail {
  position: relative;
  z-index: 9;
  background-color: #fff;
  height: 100vh;
}
.detail-nav {
  position: relative;
  z-index: 9;
  background-color: #fff;
}
.content {
  height: calc(100% - 44px - 49px);
}
</style>
