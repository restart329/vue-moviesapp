<template>
<div class="mine">
    <div class="shows" v-if="show" @click="cancel">
        {{texts}}
    </div>
    <div class="login_body">
                    <div>
                        <input class="login_text" type="text" placeHolder="账户名/手机号/Email" v-model="username">
                    </div>
                    <div>
                        <input class="login_text" type="password" placeHolder="请输入您的密码" v-model="password">
                    </div>
                    <div class="login_btn">
                        <input type="submit" value="登录" @click="btnclick">
                    </div>
                    <div class="login_link">
                        <a href="#">立即注册</a>
                        <a href="#">找回密码</a>
                    </div>
                </div>
</div>
</template>

<script>
export default {
  name: 'Login',
  data () {
    return {
      username: '',
      password: '',
      texts: '',
      show: false
    }
  },
  methods: {
    btnclick () {
      this.show = true
      if (this.username === 'admin' && this.password === 'admin') {
        this.texts = 'hello world'
      } else {
        this.texts = '账号或密码错误'
      }
    },
    cancel () {
      this.show = false
    }
  }
}
</script>

<style>
#content .login_body{  width:100%;position: relative;}
.login_body .login_text{ width:100%; height: 40px; border:none; border-bottom: 1px #ccc solid; margin-bottom: 5px; outline: none; text-indent: 10px;}
.login_body .login_btn{ height:50px; margin:10px;}
.login_body .login_btn input{ width:100%; height:100%; background:#e54847; border-radius: 3px; border:none; display: block; color:white; font-size: 20px;}
.login_body .login_link{ display: flex; justify-content:space-between;}
.login_body .login_link a{ text-decoration: none; margin:0 5px; font-size: 12px; color:#e54847;}
#content .mine{position: relative;width: 100%;height: 100%;}
.mine .shows{width: 150px;line-height: 100px;position: absolute;top: 50%;left: 50%;background-color: gray;margin-left: -75px;margin-top: -50px;text-align: center;opacity: 0.6;}
</style>
