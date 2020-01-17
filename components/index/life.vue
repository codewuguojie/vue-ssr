<template>
  <div class="m-life">
    <el-row>
      <el-col :span="14">
        <slider/>
      </el-col>
      <el-col :span="4">
        <div class="m-life-pic"/>
      </el-col>
      <el-col :span="6">
        <div v-if="!user" class="m-life-login">
          <h4><img
            src="//s0.meituan.net/bs/fe-web-meituan/2d05c2b/img/avatar.jpg"
            alt=""></h4>
          <p class="m-life-login-name">Hi！你好</p>
          <p><nuxt-link to="/register"><el-button
            round
            size="medium">注册</el-button></nuxt-link></p>
          <p><nuxt-link to="/login"><el-button
            round
            size="medium">立即登录</el-button></nuxt-link></p>
        </div>
        <div v-if="user" class="m-life-login">
          <h4><img src="//s0.meituan.net/bs/fe-web-meituan/e3064a3/img/head-img.png"></h4>
          <p class="m-life-login-name">{{user}}</p>
          <template>
            <el-row :span="24">
              <el-col :span="8">
                <dl>
                  <dt><i class="el-icon-s-order"></i></dt>
                  <dd>我的订单</dd>
                </dl>
              </el-col>
              <el-col :span="8">
                <dl>
                  <dt><i class="el-icon-star-on"></i></dt>
                  <dd>我的收藏</dd>
                </dl>
              </el-col>
              <el-col :span="8">
                <dl>
                  <dt><i class="el-icon-s-ticket"></i></dt>
                  <dd>抵用券</dd>
                </dl>
              </el-col>
              <el-col :span="8">
                <dl>
                  <dt><i class="el-icon-s-finance"></i></dt>
                  <dd>余额</dd>
                </dl>
              </el-col>
              <el-col :span="8">
                <dl>
                  <dt><i class="el-icon-more"></i></dt>
                  <dd>更多</dd>
                </dl>
              </el-col>
            </el-row>
          </template>
        </div>
      </el-col>
    </el-row>
    <el-row>
      <el-col :span="7">
        <div class="m-life-hotel"/>
      </el-col>
      <el-col :span="7">
        <div class="m-life-music"/>
      </el-col>
      <el-col :span="4">
        <div class="m-life-coop"/>
      </el-col>
      <el-col :span="6">
        <div class="m-life-downapp">
          <img
            src="//s1.meituan.net/bs/fe-web-meituan/60ac9a0/img/download-qr.png"
            alt="下载APP">
          <p>美团APP手机版</p>
          <h4><span class="red">1元起</span><em class="gary">吃喝玩乐</em></h4>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import Slider from './slider.vue'
export default {
  components: {
    Slider
  },
  data(){
    return{
      user:''
    }
  },
  async mounted() {
    const {status,data:{user}} = await this.$axios.get('/users/getUser')
    if(status===200){
      this.user = user
    }
  }
}
</script>


<style lang="scss">
    @import "@/assets/css/index/life.scss";
</style>
