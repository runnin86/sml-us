<template>
<meta name="viewport" content="width=device-width,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no"/>
<div class="content login" transition="pushtop" style="background-color:white">
  <div class="content-block">
    <img src="/img/new/logo.png" height="80" style="display:block;margin:0 auto;">
    <div class="login-input">
      <input style="-webkit-appearance:button;" type="number" v-model="userName" placeholder="用户名">
      <input style="-webkit-appearance:button;" type="password" v-model="password" placeholder="密码">
    </div>
  </div>
  <div class="submit-button" style="margin-top:1.2rem">
    <button class="button button-big button-fill"
      @click="login()"
      style="border-radius: 0.1rem">登录</button>
  </div>
  <div class="pull-right" style="font-size:0.68rem;margin:1.2rem;">
    <a @click="forgetPwd()">忘记密码?</a>
  </div>
</div>
</template>

<script>
import {userApi} from '../../util/service'
import $ from 'zepto'
export default {
  ready () {
  },
  data () {
    return {
      title: '登录',
      path: '/' + this.$route.query.from,
      userName: window.localStorage.getItem('localUsPhone') ? window.localStorage.getItem('localUsPhone') : '',
      password: '',
      submit: false
    }
  },
  methods: {
    login () {
      window.localStorage.setItem('localUsPhone', this.userName)
      // 登录时清空公告Id对象
      window.localStorage.removeItem('globalNoticeId')
      if (!this.userName || !this.password) {
        $.toast('请输入用户名或密码')
        return
      }
      this.$http.post(userApi.login,
        {
          'userName': this.userName,
          'password': this.password
        })
      .then(({data: {code, msg, info}})=>{
        if (code === 1) {
          if (info.user.user_status === 0) {
            $.toast('账户未激活，充值后激活账户')
          }
          else if (info.user.user_status === 1) {
            $.toast('登录成功')
            window.localStorage.setItem('usUser', JSON.stringify(info.user))
            window.localStorage.setItem('usToken', info.token)
            window.localStorage.setItem('imageSwitch', true)
            // 调用公告处理
            this.$root.loadNotice()
            this.$route.router.go({path: this.path, replace: true})
            // 设置购物车图标
            this.$root.setCardBadge()
          }
        }
        else {
          $.toast(msg)
        }
      }).catch((e)=>{
        $.alert('服务器连接中断...')
        console.error('无法连接服务器:' + e)
      })
    },
    forgetPwd () {
      this.$route.router.go({path: '/user/forgetPwd', replace: false})
    }
  },
  watch: {
    'userName': {
      handler: function (newVal, oldVal) {
        if (this.userName && this.password) {
          this.submit = true
        }
      }
    },
    'password': {
      handler: function (newVal, oldVal) {
        if (this.userName && this.password) {
          this.submit = true
        }
      }
    }
  }
}
</script>

<style>
.sub-title {
  margin-top: 2.2rem;
  background-color: white;
  height: 2.2rem;
  font-size: .8rem;
  font-weight: normal;
  line-height: 2.2rem;
  text-align: center;
  border-bottom: 1px solid #d8d8d9;
}
.login .content-block {
  margin: 2.3rem 0 0 0;
  padding: 0 .45rem;
}
.login-label {
  font-size: .7rem;
  line-height: 1.2rem;
}
.login-input input{
  width: 90%;
  height: 2.15rem;
  font-size: .7rem;
  padding: .4rem .5rem;
  background-color: #fff;
  margin-top: .8rem;
  margin-left: auto;
  margin-right: auto;
  display: block;
  margin-bottom: .3rem;
  border: 1px solid #f0f0f0;
  border-radius: .1rem;
}
.login textarea {
  height: auto;
  resize: none;
  width: 100%;
  font-size: .7rem;
  -webkit-appearance: none;
  padding: .4rem .5rem;
  -webkit-user-select: text;
  background-color: #fff;
  border: 1px solid rgba(0,0,0,.2);
  border-radius: .1rem;
  outline: 0;
}
.login .submit-button {
  margin-top: .8rem;
  width: 90%;
  padding: 0 .45rem;
  margin-left: auto;
  margin-right: auto;
  display: block;
}
.login .submit-button button {
  background-color: #2e96ef;
  width: 100%;
  line-height: 2.1rem !important;
  height: 2.1rem !important;
}
.color {
  background-color: #2e96ef;
}
</style>
