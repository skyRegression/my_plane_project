<template>
  <el-container style="height: 100vh;">
    <el-header style="background: linear-gradient(rgba(88, 159, 226, 0.2), rgba(6, 122, 0, 1.0)) 0% 0% / cover no-repeat fixed;">
      <AppHeader />
    </el-header>
    <el-main>
      <AppMain />
    </el-main>
  </el-container>
</template>

<script>
  import AppHeader from './AppHeader.vue';
  import AppMain from './AppMain.vue'

  export default {
    components: {
      AppHeader,
      AppMain,
    },
  };
</script>

<style scoped>
  .el-header {
    height: 15%;
    box-shadow: 0 2px 20px rgba(0, 0, 0, 0.3);
  }
  .el-main {
    height: 85%;
    box-shadow: 0 -2px 20px rgba(0, 0, 0, 0.3);
  }
</style>

