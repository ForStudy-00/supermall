<template>
  <div class="goods-item" @click="itemClick">
    <img :src="showImage" alt @load="imageLoad" />
    <div class="goods-info">
      <p>{{goodsItem.title}}</p>
      <span class="price">{{goodsItem.price}}</span>
      <span class="collect">{{goodsItem.cfav}}</span>
    </div>
  </div>
</template>
<script>
export default {
  name: 'GoodsListItem',
  props: {
    goodsItem: {
      type: Object,
      default () {
        return {}
      }
    }
  },
  computed: {
    showImage () {
      return this.goodsItem.image || this.goodsItem.show.img
    },
  },
  methods: {
    imageLoad () {
      if (this.$route.path.indexOf('/home')) {
        this.$bus.$emit('itemImageLoad')
      }else if(this.$route.path.indexOf('/detail')){
        this.$bus.$emit('detailImageLoad')
      }
    },
    itemClick () {
      // console.log("跳转到详情页");
      this.$router.push("/detail/" + this.goodsItem.iid)
    }
  },
}
</script>
<style>
.goods-item {
  width: 48vw;
  position: relative;
  padding-bottom: 40px;
}
.goods-item img {
  width: 100%;
  border-radius: 5px;
}
.goods-info {
  position: absolute;
  bottom: 5px;
  left: 0;
  right: 0;
  font-size: 12px;
  text-align: center;
  overflow: hidden;
}
.goods-info p {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  margin-bottom: 3px;
}
.goods-info .price {
  color: var(--color-high-text);
  margin-right: 20px;
}
.goods-info .collect {
  position: relative;
}
.goods-info .collect::before {
  context: '';
  position: absolute;
  left: -15px;
  top: -1px;
  width: 14px;
  height: 14px;
  background: url('../../../assets/images/common/collect.svg') 0 0/14px 14px;
}
</style>