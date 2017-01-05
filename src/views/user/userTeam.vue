<template>
<div class="content team" transition="bounce">
  <header class="bar bar-nav">
    <a class="button button-link button-nav pull-left" v-link="{path: '/user', replace: true}">
    <span class="icon icon-left" style="color:white"></span>
    </a>
    <h1 class="title color" v-text="title" style="color:white"></h1>
  </header>
  <div class="list-block">

    <ul>
      <li class="item-content2 item-link"  @click="showInfo('one',$event)" style="margin:0;padding:0;border:none;">
        <div class="item-inner">
          <div class="item-title2">
            一级用户
          </div>
          <div class="item-title2">
            <img src="/img/我的团队/人数icon.png" width="24" height="12"
              style="margin-left:-1.8rem;margin-right:0.1rem;margin-bottom:-0.01rem;">
            {{oneLevelNum > 0 ? oneLevelNum : ''}}
          </div>
          <div class="item-title2">
            <img src="/img/我的团队/销量icon.png" width="18" height="19"
              style="margin-right:0.4rem;margin-bottom:-0.16rem;">
            {{oneLevelFlow | currency '¥'}}
          </div>
        </div>
      </li>
      <li class="item-content2" id="one" style="display: none;font-size:0.6rem;">
        <div class="item-inner fcfcfc"
          v-for="u in oneLevelUsers | orderBy 'user_id' -1">
          <div class="item-title2">
            <img src="/img/我的团队/用户icon.png" width="21" height="20"
              style="margin-left:1.6rem;margin-right:0.1rem;margin-bottom:-0.16rem;">
            {{u.user_phone}}
          </div>
          <div class="item-title2">
            <img src="/img/我的团队/销量icon.png" width="18" height="19"
              style="margin-right:0.4rem;margin-bottom:-0.16rem;">
            {{u.tot | currency '¥'}}
          </div>
        </div>
      </li>
    </ul>
    <ul>
      <li class="item-content2 item-link"   @click="showInfo('two',$event)" style="margin:0;padding:0;border:none;">
        <div class="item-inner">
          <div class="item-title2">
            二级用户
          </div>
          <div class="item-title2">
            <img src="/img/我的团队/人数icon.png" width="24" height="12"
              style="margin-left:-1.8rem;margin-right:0.1rem;margin-bottom:-0.01rem;">
            {{twoLevelNum > 0 ? twoLevelNum : ''}}
          </div>
          <div class="item-title2">
            <img src="/img/我的团队/销量icon.png" width="18" height="19"
              style="margin-right:0.4rem;margin-bottom:-0.16rem;">
            {{twoLevelFlow | currency '¥'}}
          </div>
        </div>
      </li>
      <li class="item-content2" id="two" style="display: none;font-size:0.6rem;">
        <div class="item-inner fcfcfc"
          v-for="u in twoLevelUsers | orderBy 'user_id' -1">
          <div class="item-title2">
            <img src="/img/我的团队/用户icon.png" width="21" height="20"
              style="margin-left:1.6rem;margin-right:0.1rem;margin-bottom:-0.16rem;">
            {{u.user_phone}}
          </div>
          <div class="item-title2">
            <img src="/img/我的团队/销量icon.png" width="18" height="19"
              style="margin-right:0.4rem;margin-bottom:-0.16rem;">
            {{u.tot | currency '¥'}}
          </div>
        </div>
      </li>
    </ul>
    <ul>
      <li class="item-content2 item-link"   @click="showInfo('three',$event)" style="margin:0;padding:0;border:none;">
        <div class="item-inner">
          <div class="item-title2">
            三级用户
          </div>
          <div class="item-title2">
            <img src="/img/我的团队/人数icon.png" width="24" height="12"
              style="margin-left:-1.8rem;margin-right:0.1rem;margin-bottom:-0.01rem;">
            {{threeLevelNum > 0 ? threeLevelNum : ''}}
          </div>
          <div class="item-title2">
            <img src="/img/我的团队/销量icon.png" width="18" height="19"
              style="margin-right:0.4rem;margin-bottom:-0.16rem;">
            {{threeLevelFlow | currency '¥'}}
          </div>
        </div>
      </li>
      <li class="item-content2" id="three" style="display: none;font-size:0.6rem;">
        <div class="item-inner fcfcfc"
          v-for="u in threeLevelUsers | orderBy 'user_id' -1">
          <div class="item-title2">
            <img src="/img/我的团队/用户icon.png" width="21" height="20"
              style="margin-left:1.6rem;margin-right:0.1rem;margin-bottom:-0.16rem;">
            {{u.user_phone}}
          </div>
          <div class="item-title2">
            <img src="/img/我的团队/销量icon.png" width="18" height="19"
              style="margin-right:0.4rem;margin-bottom:-0.16rem;">
            {{u.tot | currency '¥'}}
          </div>
        </div>
      </li>
    </ul>
    <br>
    <div>
      <div style="width:100%;height:100%;padding-left:1rem;padding-right:1rem;">
        <img src="/img/我的团队/温馨提示.png" height="100%" width="100%">
      </div>
    </div>
  </div>
</div>
</template>

<script>
import {userApi} from '../../util/service'
import $ from 'zepto'

export default {
  ready () {
    this.$http.post(userApi.team, {}, {
      headers: {
        'x-token': window.localStorage.getItem('usToken')
      },
      emulateJSON: true
    })
    .then(({data: {code, msg, result}})=>{
      if (code === 1) {
        console.log(result)
        this.$set('oneLevelNum', result.onelevel.totalperson)
        this.$set('twoLevelNum', result.twolevel.totalperson)
        this.$set('threeLevelNum', result.threelevel.totalperson)
        this.$set('oneLevelUsers', result.onelevel.person)
        this.$set('twoLevelUsers', result.twolevel.person)
        this.$set('threeLevelUsers', result.twolevel.person)
        this.$set('oneLevelFlow', result.onelevel.totalsale)
        this.$set('twoLevelFlow', result.twolevel.totalsale)
        this.$set('threeLevelFlow', result.threelevel.totalsale)
      }
      else {
        $.toast(msg)
      }
    }).catch((e)=>{
      console.error('获取我的团队失败:' + e)
    })
  },
  data () {
    return {
      title: '我的团队',
      oneLevelNum: 0,
      twoLevelNum: 0,
      threeLevelNum: 0,
      oneLevelUsers: [],
      twoLevelUsers: [],
      threeLevelUsers: [],
      oneLevelFlow: 0,
      twoLevelFlow: 0,
      threeLevelFlow: 0
    }
  },
  methods: {
    showInfo (id, e) {
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
.team .content-padded {
  margin: 2.2rem 0 0 0;
  height: 100%;
  padding: .4rem .4rem;
}
.color {
  background-color: #2e96ef;
}
.img_full {
  width: 100%;
  height: 100%;
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
.fcfcfc {
  background-color: #fcfcfc;
}
.list-block .item-content2 {
  box-sizing: border-box;
  min-height: 2.2rem;
  display: -webkit-box;
  display: -webkit-flex;
  display: flex;
  -webkit-box-pack: justify;
  -webkit-justify-content: space-between;
  justify-content: space-between;
  -webkit-box-align: center;
  -webkit-align-items: center;
  align-items: center;
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
.list-block ul:before {
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  bottom: auto;
  right: auto;
  height: 1px;
  width: 100%;
  background-color: #e7e7e7;
  display: block;
  z-index: 15;
  -webkit-transform-origin: 50% 0%;
  transform-origin: 50% 0%;
}
.list-block .item-inner:before {
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  bottom: auto;
  right: auto;
  height: 1px;
  width: 100%;
  background-color: #e7e7e7;
  display: block;
  z-index: 15;
  -webkit-transform-origin: 50% 0%;
  transform-origin: 50% 0%;
}
.total{
  background: white;
  padding:0.5rem 0;
}
.total span{
  display: block;
  width: 100%;
  text-align: center;
  color: #666666;
  line-height: 1.2rem;
  font-size: 0.8rem;
}
</style>
