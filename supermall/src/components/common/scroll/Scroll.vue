<template>
  <div class="wrapper" ref="wrapper">
    <div class="content">
      <slot></slot>
    </div>
  </div>
</template>
<script>
import BScroll from 'better-scroll'
export default {
  name: "Scroll",
  data () {
    return {
      scroll: null
    }
  },
  props: {
    probeType: {
      type: Number,
      default: 0
    },
  },
  mounted () {
    this.scroll = new BScroll(this.$refs.wrapper, {
      click: true,
      probeType: this.probeType,
      // pullUpLoad: this.pullUpLoad
    }),
      this.scroll.on('scroll', (position) => {
        this.$emit('scroll', position)
      })
  },
  methods: {
    scrollTo (x, y, time = 500) {
      this.scroll.scrollTo(x, y, time)
    },
    refresh () {
      this.scroll.refresh()
    }
  },
}
</script>
<style>
</style>