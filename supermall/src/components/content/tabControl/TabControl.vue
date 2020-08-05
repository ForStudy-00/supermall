<template>
  <div class="tab-control">
      <div v-for="item in titles" :key="item">
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
  }
}
</script>
<style>
</style>