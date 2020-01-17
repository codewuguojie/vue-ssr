<template>
    <div class="page-register">
      <article class="header">
        <header>
          <a href="/" class="site-logo"></a>
          <span class="login">
            <em class="bold">已有美团账号？</em>
            <a href="/login">
              <el-button type="primary" size="small">登录</el-button>
            </a>
          </span>
        </header>
      </article>
      <section>
        <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
          <el-form-item label="昵称" prop="name">
            <el-input v-model="ruleForm.name"></el-input>
          </el-form-item>
          <el-form-item label="邮箱" prop="email">
            <el-input v-model="ruleForm.email"></el-input>
            <el-button size="mini" round @click="sendMsg()">发送验证码</el-button>
            <span class="status">{{statusMsg}}</span>
          </el-form-item>
          <el-form-item label="验证码" prop="code">
            <el-input v-model="ruleForm.code" maxlength="4"></el-input>
          </el-form-item>
          <el-form-item label="密码" prop="pwd">
            <el-input v-model="ruleForm.pwd" type="password"></el-input>
          </el-form-item>
          <el-form-item label="确认密码" prop="cpwd">
            <el-input v-model="ruleForm.cpwd" type="password"></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="register()">同意以下协议并注册</el-button>
            <div class="error">{{error}}</div>
          </el-form-item>
          <el-form-item>
            <a class="f1" href="https://rules-center.meituan.com/rules-detail/4" target="_blank">《美团点评用户服务协议》</a>
            <a class="f1" href="https://rules-center.meituan.com/rules-detail/2" target="_blank">《美团点评隐私政策》</a>
          </el-form-item>
        </el-form>
      </section>
    </div>
</template>

<script>
  import CryptoJs from 'crypto-js'
    export default {
        name: "register",
        layout:'blank',
        data(){
          return{
            error:'',
            statusMsg:'',
            ruleForm:{
              name:'',
              code:'',
              pwd:'',
              cpwd:'',
              email:''
            },
            rules:{
              name:[
                {
                  required:true,type:'string',message:'请输入昵称',trigger:'blur'
                }
              ],
              email:[
                {
                  required:true,type:'email',message:'请输入正确的邮箱',trigger:'blur'
                }
              ],
              pwd:[
                {
                  required:true,message:'创建密码',trigger:'blur'
                }
              ],
              cpwd:[
                {
                  required:true,message:'确认密码',trigger:'blur'
                },{
                  validator:(rule,value,callback)=>{
                    if(value===''){
                      callback(new Error('请再次输入密码'))
                    } else if(value!==this.ruleForm.pwd){
                      callback(new Error('两次输入密码不一样'))
                    } else {
                      callback()
                    }
                  },
                  trigger:'blur'
                }
              ],
            }
          }
        },
        methods:{
          sendMsg(){
            let that = this
            let namePass
            let emailPass
            if(that.timerid){
              return false
            }
            this.$refs['ruleForm'].validateField('name',(volid) => {
              namePass = volid
            })
            that.statusMsg = ''
            if(namePass){
              return false
            }
            this.$refs['ruleForm'].validateField('email',(voild) => {
              emailPass = voild
            })
            if(!namePass&&!emailPass){
              that.$axios.post('/users/verify',{
                username:encodeURIComponent(that.ruleForm.name),
                email:that.ruleForm.email
              }).then(({status,data}) =>{
                if(status===200&&data&&data.code===0){
                  let count = 60;
                  that.statusMsg = `验证码已发送，剩余${count--}秒`
                  that.timerid = setInterval(function () {
                    that.statusMsg = `验证码已发送，剩余${count--}秒`
                    if(count===-1){
                      clearInterval(that.timerid)
                      that.statusMsg = ''
                    }
                  },1000)
                } else {
                  that.statusMsg = data.msg
                }
              })
            }
          },
          register(){
            let that = this
            this.$refs['ruleForm'].validate((voild)=>{
              if(voild){
                that.$axios.post('/users/signup',{
                  username:encodeURIComponent(that.ruleForm.name),
                  password:CryptoJs.MD5(that.ruleForm.pwd).toString(),
                  email:that.ruleForm.email,
                  code:that.ruleForm.code
                }).then(({status,data})=>{
                  if(status===200){
                    if(data&&data.code===0){
                      location.href = '/login'
                    } else {
                      that.error = data.msg
                    }
                  } else {
                    that.error = `服务器出错,错误码${status}`
                  }
                  setTimeout(function () {
                    that.error = ''
                  },1500)
                })
              }
            })
          }
        }
    }
</script>

<style lang="scss">
@import "@/assets/css/register/index";
</style>
