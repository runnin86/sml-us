<template>
<div class="content recharge" transition="bounce">
  <header class="bar bar-nav">
    <a class="button button-link button-nav pull-left"
      v-link="{path: '/user/bill', query: { pageTransition: 'fade' }, replace: true}">
    <span class="icon icon-left" style="color:white"></span>
    </a>
    <h1 class="title color" v-text="title" style="color:white"></h1>
  </header>
  <div class="list-block">
    <ul v-for="t in list" track-by="$index">
      <li class="item-content2">
        <div class="item-inner">
          <div class="item-title2" style="width:40%;">
              {{t.recharge_date}}
          </div>
          <div class="item-title2"
            :style="{color:t.recharge_flag==='0'?'red':''}"
            style="width:30%;text-align:center;">
            <!-- {{t.recharge_remarks}} -->
            {{t.recharge_flag==='0'?'失败':'成功'}}
          </div>
          <div class="item-title2" style="width:20%;text-align:right;">
            {{t.recharge_money}}
          </div>
        </div>
      </li>
    </ul>
  </div>
  <div v-cloak v-if="showWarning" style="width:100%;height:100%;text-align:center;">
    <div>
      <img src="/img/专家方案/温馨提示.png" height="24" width="152">
    </div>
    <div style="font-size:0.38rem;color:#A9A9A9;">
      您还没有充值记录可供查看
    </div>
  </div>
</div>
</template>

<script>
import {userApi} from '../../../util/service'
import $ from 'zepto'

export default {
  ready () {
    $.showIndicator()
    this.$http.post(userApi.myrecharge, {}, {
      headers: {
        'x-token': window.localStorage.getItem('usToken')
      },
      emulateJSON: true
    })
    .then(({data: {code, msg, result}})=>{
      if (code === 1) {
        // console.log(result)
        if (result.length > 0) {
          this.$set('showWarning', false)
          this.$set('list', result)
        }
        else {
          this.$set('showWarning', true)
        }
      }
      else {
        $.toast(msg)
      }
      $.hideIndicator()
    }).catch((e)=>{
      console.error('获取我的账单(充值记录)失败:' + e)
    })
  },
  data () {
    return {
      title: '充值记录',
      list: [],
      showWarning: false
    }
  },
  methods: {
  }
}
</script>

<style scoped>
.recharge .content-padded {
  margin: 2.2rem 0 0 0;
  height: 100%;
  padding: .4rem .4rem;
}
.color {
  background-color: #2e96ef;
}
.list-block {
  margin: 2.2rem 0;
  height: 2.9rem;
  font-size:0.66rem;

  ul {
    height: 2.9rem;
  }

  .item-content {
    margin-top: .7rem;
    height: inherit;
  }
}
.list-block .item-title2 {
  -webkit-flex-shrink: 1;
  -ms-flex: 0 1 auto;
  -webkit-flex-shrink: 1;
  flex-shrink: 1;
  /*white-space: nowrap;*/
  position: relative;
  /*overflow: hidden;*/
  /*text-overflow: ellipsis;*/
  max-width: 100%;
  padding-left: 0.4rem;
}
</style>
