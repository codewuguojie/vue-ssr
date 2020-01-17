<template>
  <div class="search-panel">
    <el-row class="m-header-searchbar">
      <el-col :span="3" class="left">
        <nuxt-link to="/"><img src="//s0.meituan.net/bs/fe-web-meituan/e5eeaef/img/logo.png" alt="美团"></nuxt-link>
      </el-col>
      <el-col :span="15" class="center">
        <div class="wrapper">
          <el-input placeholder="搜索商家或地点" v-model="search" @focus="focus()" @blur="blur()" @input="getList()" />
          <button class="el-button el-button--primary"><i class="el-icon-search"></i></button>
          <dl class="hotPlace" v-if="isHotPlace">
            <dt>热门搜索</dt>
            <dd v-for="(item,index) in $store.state.home.hotPlace.slice(0,5)">
              <a :href="'/products?keyword='+encodeURIComponent(item.name)">{{ item.name }}</a>
            </dd>
          </dl>
          <dl class="searchList" v-if="isSearchList">
            <dd v-for="(item,index) in searchList">
              <a :href="'/products?keyword='+encodeURIComponent(item.name)">{{ item.name }}</a>
            </dd>
          </dl>
        </div>
        <p class="suggset">
          <a style="margin: 0 5px 0 0" :href="'/products?keyword='+encodeURIComponent(list.name)" v-for="(list,index) in $store.state.home.hotPlace.slice(0,6)">{{ list.name }}</a>
        </p>
        <ul class="nav">
          <li>
            <nuxt-link to="/" class="takeout">美团外卖</nuxt-link>
          </li>
          <li>
            <nuxt-link to="/" class="movie">猫眼电影</nuxt-link>
          </li>
          <li>
            <nuxt-link to="/" class="hotel">美团酒店</nuxt-link>
          </li>
          <li>
            <nuxt-link to="/" class="apartment">民宿/公寓</nuxt-link>
          </li>
          <li>
            <nuxt-link to="/" class="business">商家入驻</nuxt-link>
          </li>
        </ul>
      </el-col>
      <el-col :span="6" class="right">
        <ul class="security">
          <li><i class="refund"></i><p class="txt">随时退</p></li>
          <li><i class="single"></i><p class="txt">不满意免单</p></li>
          <li><i class="overdue"></i><p class="txt">过期退</p></li>
        </ul>
      </el-col>
    </el-row>
  </div>
</template>

<script>
  import _ from 'lodash'
    export default {
        name: "search",
        data(){
          return{
            search:'',
            isFocus:false,
            hotPlace:[],
            searchList:[]
          }
        },
        computed:{
          isHotPlace:function () {
            return this.isFocus&&!this.search
          },
          isSearchList:function () {
            return this.isFocus&&this.search
          }
        },
        methods:{
          focus(){
            this.isFocus = true
          },
          blur(){
            let that = this
            setTimeout(function () {
              that.isFocus = false
            },200)
          },
          getList(){
            let that = this
            setTimeout(async  function () {
              let city = that.$store.state.geo.position.city.replace('市','')
              that.searchList = []
              let {status,data:{top}} = await that.$axios.get('/search/top',{
                params:{
                  input:that.search,
                  city
                }
              })
              that.searchList = top.slice(0,10)
            },300)
          }
        }
    }
</script>

<style lang="scss">

</style>
