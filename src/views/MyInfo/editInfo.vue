<template>
  <div>
    <div class="content">
      <div class="header">
        <span @click="$router.go(-1)" class="back"><img style=" width:0.66rem;height: 1.175rem;display: inline-block;" src="./img/head-back.png" alt=""></span>
        <p>基本信息</p>
      </div>
      <div class="myIntro">
        <div class="intro-item" @click="$router.push({path:'/AddNick'})"><!--@click="AddNick" -->
          <div class="intro-item-l">昵称</div>
          <div class="intro-item-r">
            {{userInfoDetail.name}}
            <span><img src="./img/arrow.png" alt=""></span>
          </div>
        </div>
        <div class="intro-item">
          <div class="intro-item-l">所在地</div>
          <div class="intro-item-r" style="position:relative">
            <input class="inputer" placeholder="省份，城市" v-model="cityVal" readonly="readonly"/>
            <div class="sel">
              <popup-picker :data="cityList" :columns="3" v-model="cityVal" ref="cityPicker" ></popup-picker>
            </div>
          </div>
        </div>
        <div class="intro-item">
          <div class="intro-item-l">类型</div>
          <div class="intro-item-r" style="position:relative">
            <input class="inputer" placeholder="资金资产类型" v-model="AFType" readonly="readonly"/>
            <div class="sel">
              <popup-picker :data="AFList" :columns="1" v-model="AFType" ref="AFTypePicker" ></popup-picker>
            </div>
          </div>
        </div>
      </div>
      <div class="myIntro" style="margin-top: 0.6rem;">
        <div class="intro-item self-intro" @click="AddMyIntro">
          <div class="intro-item-l">个人简介</div>
          <div class="intro-item-r">
            {{userInfoDetail.introduction}}
            <span><img src="./img/arrow.png" alt=""></span>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<script>

import Lib from '@/assets/js/Lib'

import { Loading, XButton, Confirm,Cell,PopupPicker } from 'vux'

import MainNav from '@/components/mainNav'

import myHead from '@/components/myHead'

import $ from 'jquery'


export default {
  name: 'Home', 
  components: {
    MainNav, Loading, XButton, Confirm,myHead, PopupPicker,Cell

  },
  data () {
    return {
      img_id: '',
      noAvatar:true,
      intro:'',
      cityVal: [],
      cityList: Lib.M.cityList,
      AFType:[],
      AFList: Lib.M.AFList,
      myData:{},
      userInfoDetail:{},
    }
  },
  watch: {
    // 如果 `cityVal` 发生改变，这个函数就会运行
    cityVal: function (newQuestion) {
      console.log(this.cityVal)
      this.submitMyInfo();
    },
    // 如果 `AFType` 发生改变，这个函数就会运行
    AFType: function (newQuestion) {
      console.log(this.AFType)
      this.submitMyInfo();
    }
  },
  computed:{

  },
  mounted(){
    this.localUserInfo = localStorage;
    this.getMyInfo();
  },
  methods: {
    AddNick(){
      this.$router.push('./AddNick')
    },
    AddMyIntro(){
      this.$router.push('./AddMyIntro')
    },
    getUserType(key){
      return Lib.M.getUserType(key);
    },
    getMyInfo(){
      var self = this;
      Lib.M.ajax({
        type:'post',
        url: "/user/getUserInfoDetail",
        data:{
          'userId':self.localUserInfo.userId,
        },
        success:function (res) {
          if(res.code==200){
            self.myData = res.data;
            if(res.data.userInfoDetail !== null){
              self.userInfoDetail = res.data.userInfoDetail;
              self.cityVal[0] = self.userInfoDetail.address.split(' ')[0];
              self.AFType[0] = self.getUserType(self.userInfoDetail.type);
            }
          }else{
            self.$vux.toast.text(res.error, 'middle');
          }
        }
      })
    },
    submitMyInfo(){
      var self = this;
      /* 解析AFType */
      var AFType = 0;
      var f = JSON.parse(localStorage.userType);
      for(let i in f){
        if(f[i].label == self.AFType) 
          AFType = f[i].key;
      }
      Lib.M.ajax({
        type:'post',
        url: "/user/submitUserInfoDetail",
        data:{
          'userId':self.localUserInfo.userId,
          'name':'',
          'address':self.cityVal.join(','),
          'type': AFType,
          'introduction':'',
        },
        success:function (res) {
          if(res.code==200){
            /*self.$vux.toast.text('修改成功', 'middle');*/
          }else{
            self.$vux.toast.text(res.error, 'middle');
          }
        }
      })
    },





  }
}
</script>

<style lang="less" scoped>
  ::-webkit-input-placeholder { /* WebKit, Blink, Edge */
    color:   #333333;
    font-size: 0.875rem;
  }
  :-moz-placeholder { /* Mozilla Firefox 4 to 18 */
    color:    #333333;
    font-size: 0.875rem;
  }
  ::-moz-placeholder { /* Mozilla Firefox 19+ */
    color:    #333333;
    font-size: 0.875rem;
  }
  :-ms-input-placeholder { /* Internet Explorer 10-11 */
    color:    #333333;
    font-size: 0.875rem;
  }
  .content{
    width: 100%;
    background: #EFEFF4;
    min-height: 40rem;
    position: absolute;
    left: 0;
    top: 0;
    .header{
      width:100%;
      height:2.75rem;
      background:#fff;
      margin: 0 auto;
      font-size: 1.065rem;
      color: #000;
      text-align: center;
      position: relative;
      line-height: 2.75rem;
      font-weight: 600;
      span{
        position: absolute;
        height: 100%;
        left: 1.5rem;
        display: flex;
        align-items: center;
        justify-content: center;
      }
    }
    .myIntro{
      background: #fff;
      margin-top: 0.6rem;
      .intro-item{
        height: 2.72rem;
        line-height: 2.72rem;
        display: flex;
        border-bottom: 0.06rem solid #E6E7EB;
        padding:0 1.1rem;
        div{
          text-align: left;
          font-size: 0.875rem;
        }
        .intro-item-l{
          width: 0;
          flex: 1;
          color: #999999;
        }
        .intro-item-r{
          position: relative;
          flex: 3;
          color: #333333;
          span{
            float: right;
            img{
              width: 0.44rem;
              height: 0.75rem;
            }
          }
          .inputer{
            outline: none;
            border: none;
            font-size: 0.94rem;
            height: 2.75rem;
            text-align: left;
            margin-right: 1rem;
          }
          .sel{
            background: url('./img/arrow.png') no-repeat;
            background-size: 0.44rem 0.75rem;
            background-position: 100% center;
            width: 100%;
            position:absolute;
            top:0;
            right:0;
            bottom:0;
            overflow: hidden;
          }
        }
      }
      .self-intro{
        height: 5rem;
        border:none;
        .intro-item-r{
          line-height: 1.5rem;
          margin-top: 0.6rem;
        }
      }
    }


  }
  
</style>
