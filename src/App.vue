<template>
  <div id="app">
    <router-view/>
    <main-tab-bar/>
  </div>
</template>

<style>
@import url("assets/css/base.css");
</style>
<script>
import MainTabBar from "components/content/mainTabbar/MainTabBar";
export default {
  components: {MainTabBar}
}
</script>
