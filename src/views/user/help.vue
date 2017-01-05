<template>
<div class="content invite" transition="bounce">
  <header class="bar bar-nav">
    <a class="button button-link button-nav pull-left"
      @click="this.$root.back()">
    <span class="icon icon-left" style="color:white"></span>
    </a>
    <h1 class="title color" v-text="title" style="color:white"></h1>
  </header>
  <div class="list-block" style="margin: 2.35rem 0">
    <!-- <ul>
      <li class="item-content item-link" @click="showMsg('registerHelp',$event)">
        <div class="item-inner">
          <div class="item-title">注册不成功</div>
        </div>
      </li>
      <li class="item-content" id="registerHelp" style="display: none;background-color: #EEEEEE;">
        <div class="item-inner">
          <div class="helpMsgText">
            注册不成功注册不成功注册不成功注册不成功注册不成功注册不成功注册不成功
          </div>
        </div>
      </li>
    </ul>
    <ul>
      <li class="item-content item-link" @click="showMsg('rechargeHelp',$event)">
        <div class="item-inner">
          <div class="item-title">充值问题</div>
        </div>
      </li>
      <li class="item-content" id="rechargeHelp" style="display: none;background-color: #EEEEEE;">
        <div class="item-inner">
          <div class="helpMsgText">
            注充值问题充值问题充值问题充值问题充值问题充值问题充值问题充值问题充值问题
            充值问题
            充值问题充值问题充值问题充值问题充值问题
          </div>
        </div>
      </li>
    </ul> -->
    <ul>
      <li class="item-content item-link" @click="showMsg('withdrawHelp',$event)">
        <div class="item-inner">
          <div class="item-title">提现问题</div>
        </div>
      </li>
      <li class="item-content" id="withdrawHelp" style="display: none;background-color: white; padding-bottom:1rem;padding-top:.6rem;">
        <div class="item-inner">
          <div class="helpMsgText">
            1.在个人中心点击提现按钮可以对余额进行提现，提现金额10元起。</br>
            2.流水返佣部分按照当月产生次月提取的规则，结算日为每月18号，金额会自动转入用户的余额账户。</br>
            3.每次提现将收取3%的手续费。</br>
          </div>
        </div>
      </li>
    </ul>
  </div>
</div>
</template>

<script>
// import $ from 'zepto'

export default {
  ready () {
  },
  data () {
    return {
      title: '帮助'
    }
  },
  methods: {
    showMsg (id, e) {
      if (document.getElementById(id).style.display === 'block') {
        document.getElementById(id).style.display = 'none'
      }
      else {
        document.getElementById(id).style.display = 'block'
      }
    }
  }
}
</script>

<style scoped>
.list-block {
  height: 2.9rem;
  font-size: 0.72rem;

  ul {
    height: 2.9rem;
  }

  .item-content {
    margin-top: .7rem;
    height: inherit;
  }
}
.helpMsgText {
  font-size: 0.7rem;
  color: #666666;
  line-height: 1.2rem;
}
.color {
  background-color: #2e96ef;
}
a:link {
  color: #000000;
  text-decoration: none;
}
a:visited {
  color: #000000;
  text-decoration: none;
}
a:hover {
  color: #999999;
  text-decoration: underline;
}
</style>
