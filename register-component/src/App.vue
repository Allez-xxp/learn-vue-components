<template>
  <div id="app">
    <div class="container"></div>
    <!-- <search /> -->
    <router-view/>
  </div>
</template>
<script>
// import Search from './components/Search';
export default {
  components:{
    // Search
  }
}
</script>
<style>
* {
  margin: 0;
  padding: 0;
}
#app {
  padding: 0 15px;
}
</style>
