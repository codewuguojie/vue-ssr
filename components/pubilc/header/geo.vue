<template>
  <div class="m-geo">
    <i class="el-icon-location">{{ $store.state.geo.position.city }}</i>
    <nuxt-link class="changeCity" to="/changeCity">切换城市</nuxt-link>
    [香河 廊坊 天津]
  </div>
</template>

<script>
    export default {
        name: "geo",
        mounted() {
          console.log(123,this.$store.state.geo)
        }
    }
</script>

<style lang="scss">

</style>
