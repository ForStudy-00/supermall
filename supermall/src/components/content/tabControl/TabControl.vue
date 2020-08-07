<template>
  <div class="tab-control">
    <div
      class="tab-control-item"
      v-for="(item,index) in titles"
      :key="item"
      :class="{active:index===currentIndex}"
      @click="itemClick(index)"
    >
      <span>{{item}}</span>
    </div>
  </div>
</template>
<script>
export default {
  name: "TabControl",
  props: {
    titles: {
      type: Array,
      data () {
        return []
      },
    }
  },
  data () {
    return {
      currentIndex: 0
    }
  },
  methods: {
    itemClick (index) {
      this.currentIndex = index
      this.$emit('tabClick', index)
    },
  },
}
</script>
<style scoped>
.tab-control {
  display: flex;
  text-align: center;
  background-color: #fff;
  height: 40px;
  line-height: 40px;
  font-size: 16px;
  z-index: 1;
}
.tab-control-item {
  flex: 1;
}
.tab-control-item span {
  height: 35px;
  padding: 0 5px;
}
.active {
  color: var(--color-high-text);
}
.active span {
  border-bottom: 3px solid var(--color-tint);
}
</style>