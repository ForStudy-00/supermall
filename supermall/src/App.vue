<template>
  <div id="app">
    <mian-tab-bar></mian-tab-bar>
    <router-view/>
  </div>
</template>
<script>
require("normalize.css")
import MianTabBar from './components/content/MainTabBar'
export default {
  name:'app'
}
</script>
<style>
@import url("./assets/css/base.css");
</style>
