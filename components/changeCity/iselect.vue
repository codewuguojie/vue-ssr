<template>
    <div class="m-iselect">
      <span class="name">按省份选择：</span>
      <el-select v-model="pvalue" placeholder="省份">
        <el-option
          v-for="item in province"
          :key="item.value"
          :label="item.label"
          :value="item.value">
        </el-option>
      </el-select>
      <el-select v-model="cvalue" :disabled="!city.length" placeholder="城市">
        <el-option
          v-for="item in city"
          :key="item.value"
          :label="item.label"
          :value="item.value">
        </el-option>
      </el-select>
      <span style="margin-left: 50px">直接搜索：</span>
      <el-autocomplete
        v-model="input"
        :fetch-suggestions="querySearchAsync"
        placeholder="请输入城市中文活拼音"
        @select="handleSelect"
      ></el-autocomplete>
    </div>
</template>

<script>
    import _ from 'lodash'
    export default {
        name: "iselect",
      data(){
          return{
            province:[],
            pvalue:'',
            city:[],
            cvalue:'',
            input:'',
            cities:[]
          }
      },
      watch:{
        async pvalue(pvalue){
          let that = this
          let {status,data:{city}} =await that.$axios.get(`/geo/province/${pvalue}`)
          if(status===200){
            that.city = city.map(item => {
              return{
                value:item.id,
                label:item.name
              }
            })
            that.cvalue = ''
          }
        }
      },
      async mounted(){
          let that = this
          let {status,data:{province}} = await that.$axios.get('geo/province')
        if(status===200){
          that.province = province.map(item=>{
            return{
              value:item.id,
              label:item.name
            }
          })
        }
      },
      methods:{
        querySearchAsync:_.debounce(async function(query,cb){
          let that = this
          if(this.cities.length){
            cb(that.cities.filter(item=>item.value.indexOf(query)>-1))
          } else {
            let {status,data:{city}} = await that.$axios.get('/geo/city')
            if(status===200){
              that.cities = city.map(item=>{
                return{
                  value:item.name,
                }
              })
              cb(that.cities.filter(item=>item.value.indexOf(query)>-1))
            } else {
              cb([])
            }
          }
        },200),
        handleSelect(e){
          console.log(e)
          this.$store.state.geo.position.city = e.value
          this.$router.push({
            path:'/'
          })
        }
      }
    }
</script>

<style lang="scss">
  @import "@/assets/css/changecity/iselect.scss";
</style>
