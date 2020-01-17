<template>
  <el-container class="layout-default">
    <el-header style="height: auto;">
      <my-header/>
    </el-header>
    <el-main>
      <nuxt />
    </el-main>
    <el-footer style="height: auto;">
      <my-footer/>
    </el-footer>
  </el-container>
</template>

<script>
  import myHeader from '@/components/pubilc/header/index'
  import myFooter from '@/components/pubilc/footer/index'
  export default {
    components:{
      myHeader,
      myFooter
    }
  }
</script>

<style lang="scss">

</style>


