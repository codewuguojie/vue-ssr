<template>
  <div class="m-menu">
    <dl class="nav" @mouseleave="mouseLeave()">
      <dt>全部分类</dt>
      <dd v-for="(item,index) in $store.state.home.menu" @mouseenter="mouseEnter">
        <i :class="item.type"/>{{ item.name }}<span class="arrow"></span>
      </dd>
    </dl>
    <div class="detail" v-if="kind" @mouseenter="sover" @mouseleave="sout">
      <template v-for="(item,index) in curDetail.child">
        <h4>{{ item.title }}</h4>
        <span v-for="(subItem,index) in item.child">{{ subItem }}</span>
      </template>
    </div>
  </div>
</template>

<script>
    export default {
        name: "Emenu",
        data(){
          return{
            kind:'',
            menu:[
              {
                type:'food',
                name:'美食',
                child:[
                  {
                    title:'美食',
                    child:['代金券','甜点','饮品','火锅','自助餐','小吃快餐']
                  }
                ]
              },
              {
                type:'takeout',
                name:'外卖',
                child:[
                  {
                    title:'外卖',
                    child:['美团外卖']
                  }
                ]
              },
              {
                type:'hotel',
                name:'酒店',
                child:[
                  {
                    title:'酒店星级',
                    child:['经济型','舒适/三星','高档/四星','豪华/五星']
                  }
                ]
              }
            ]
          }
        },
      computed:{
          curDetail:function () {
            return this.$store.state.home.menu.filter((item)=> item.type===this.kind)[0]
          }
      },
      methods:{
        mouseLeave(){
          let that = this
          that._timer = setTimeout(function () {
            that.kind = ''
          },150)
        },
        mouseEnter(e){
          this.kind = e.target.querySelector('i').className
        },
        sover(){
          clearTimeout(this._timer)
        },
        sout(){
          this.kind = ''
        }
      }
    }
</script>

<style lang="scss">

</style>
