<template>
  <div id="tab-bar">
    <slot></slot>
  </div>
</template>
<script>
export default {
  name: "TabBar"
}
</script>
<style>
#tab-bar {
  display: flex;
  background-color: #f6f6f6;
  position: fixed;
  z-index: 1;
  left: 0;
  right: 0;
  bottom: 0;
  box-shadow: 0px -1px 1px rgba(0, 0, 0, 0.1);
}
</style>