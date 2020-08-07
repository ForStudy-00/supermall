<template>
  <div id="app">
    <main-tab-bar></main-tab-bar>
    <keep-alive exclude="Detail">
      <router-view />
    </keep-alive>
  </div>
</template>
<script>
require("normalize.css")
import MainTabBar from './components/content/MainTabBar'
export default {
  name: 'app',
  components: {
    MainTabBar
  }
}
</script>
<style>
@import url('./assets/css/base.css');
</style>
