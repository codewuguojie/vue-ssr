<template>
    <el-row
      :gutter="0"
      class="m-header">
      <el-col :span="5">
        <geo/>
      </el-col>
      <el-col :span="5">
        <user/>
      </el-col>
      <el-col :span="14">
        <nav-bar/>
      </el-col>
    </el-row>
</template>

<script>
  import geo from './geo'
  import user from './user'
  import navBar from './nav'
    export default {
        name: "index",
        components:{
            geo,
            user,
            navBar
        }
    }
</script>

<style lang="scss">
  @import "@/assets/css/public/layout";
</style>
