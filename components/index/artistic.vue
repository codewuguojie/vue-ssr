<template>
  <section class="m-istyle">
    <dl @mouseover="over">
      <dt>有格调</dt>
      <dd
        :class="{active:kind==='all'}"
        kind="all"
        keyword="景点">全部</dd>
      <dd
        :class="{active:kind==='part'}"
        kind="part"
        keyword="美食">约会聚餐</dd>
      <dd
        :class="{active:kind==='spa'}"
        kind="spa"
        keyword="丽人">丽人SPA</dd>
      <dd
        :class="{active:kind==='movie'}"
        kind="movie"
        keyword="电影">电影演出</dd>
      <dd
        :class="{active:kind==='travel'}"
        kind="travel"
        keyword="旅游">品质出游</dd>
    </dl>
    <ul class="ibody"
        v-loading="loading"
        element-loading-text="拼命加载中"
        element-loading-background="rgba(0, 0, 0, 0.6)"
    >
      <li
        v-for="item in cur"
        :key="item.title">
        <el-card
          :body-style="{ padding: '0px' }"
          shadow="never">
          <img
            :src="item.img"
            class="image">
          <ul class="cbody">
            <li class="title">{{ item.title }}</li>
            <li class="pos"><span>{{ item.pos }}</span></li>
            <li class="price">￥<em>{{ item.price }}</em><span>/起</span></li>
          </ul>
        </el-card>
      </li>
    </ul>
  </section>
</template>
<script>
export default {
  data() {
    return {
      kind: 'all',
      loading:true,
      list: {
        all: [],
        part: [],
        spa: [],
        movie: [],
        travel: []
      }
    }
  },
  computed: {
    cur() {
      console.log('list',this.list[this.kind])
      return this.list[this.kind]
    }
  },
  async mounted(){
    let that = this
    let {status,data:{count,pois}} = await that.$axios.get('search/resultsByKeywords',{
      params:{
        keyword:'景点',
        city:that.$store.state.geo.position.city
      }
    })
    if(status===200&&count>0){
      console.log('数据',pois)
      let r = pois.filter(item=>item.photos.length).map(item=>{
        return {
          title:item.name,
          pos:item.type.split(';')[0],
          price:item.biz_ext.cost||'暂无',
          img:item.photos[0].url,
          url:'//abc.com'
        }
      })
      that.list[that.kind] = r.slice(0,9)
      that.loading = false
    } else {
      that.list[that.kind] = []
    }
  },
  methods: {
    async over(e){
      let that = this
      if(e.target.tagName==='DD'){
        that.kind = e.target.getAttribute('kind')
        let keyword = e.target.getAttribute('keyword')
        let {status,data:{count,pois}} = await that.$axios.get('search/resultsByKeywords',{
          params:{
            keyword,
            city:that.$store.state.geo.position.city
          }
        })
        if(status===200&&count>0){
          console.log('数据',pois)
          let r = pois.filter(item=>item.photos.length).map(item=>{
            return {
              title:item.name,
              pos:item.type.split(';')[0],
              price:item.biz_ext.cost||'暂无',
              img:item.photos[0].url,
              url:'//abc.com'
            }
          })
          that.list[that.kind] = r.slice(0,9)
        } else {
          that.list[that.kind] = []
        }
      }
      console.log('kind',that.kind)
    }
  },

}
</script>
<style lang="scss">
    @import "@/assets/css/index/artistic.scss";
</style>
