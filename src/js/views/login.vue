<template>
  <div class="login-wrap">
    <div class="login-box">
      <div class="form-item">
        <label class="form-item-label"><text style="color: white">手机号</text></label>
        <input v-model="loginForm.phone" class="form-item-input" type="tel" placeholder="请输入手机号">
      </div>
      <div class="form-item">
        <label class="form-item-label"><text style="color: white">密码</text></label>
        <input v-model="loginForm.password" class="form-item-input" type="password" placeholder="请输入密码">
      </div>
      <div class="form-item-btn">
        <wxc-button
          type="blue"
          text="登录"
          @wxcButtonClicked="loginClick"></wxc-button>
      </div>
    </div>
  </div>
</template>

<script>
  import {
    WxcCell,
    WxcButton
  } from 'weex-ui'
  export default {
    name: "login",
    components:{
      WxcCell,
      WxcButton
    },
    data(){
      return {
        loginForm:{
          phone: null,
          password: null
        }
      }
    },
    methods:{
      // ?phone=xxx&password=yyy
      loginClick () {
        this.$fetch({
          method: 'GET',    // 大写
          name: 'login_phone', //当前是在apis中配置的别名，你也可以直接绝对路径请求 如：url:http://xx.xx.com/xxx/xxx
          data: this.loginForm
        }).then(resData => {
          // 成功回调
          console.log(resData)
          this.$storage.setSync('userInfo', resData)
        }, error => {
          // 错误回调
          console.log(error)
        })
      }
    }
  }
</script>

<style scoped>
  .login-wrap{
    width: 750px;
    background-color: black;
    align-items: center;
    justify-content: center;
  }
  .login-box{
    width: 700px;
    border-radius: 5px;
    border-width: 1px;
    border-color: #ababab;
  }
  .form-item{
    width: 700px;
    height: 90px;
    flex-direction: row;
    justify-content: center;
    border-bottom-width: 1px;
    border-color: white;
    padding-bottom: 20px;
  }
  .form-item-label{
    width: 150px;
    justify-content: center;
    align-items: center;
  }
  .form-item-input{
    width: 550px;
  }
  .form-item-btn{
    margin-top: 50px;
  }
</style>
