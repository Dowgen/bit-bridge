<template>
  <div>
    <div class="content" v-cloak>
<!--       <swiper loop auto height="12.44rem" dots-position="center">
        <swiper-item :key="pic.picUrl" v-for="pic in img_src">
          <img :src="pic.picUrl" @click="linkTo(pic.linkUrl)">
        </swiper-item>
      </swiper> -->
      <img src="./img/img3_news.png">
      <div class="recommend">
        <p>
          <span>倾力推荐</span>
          <span @click="lookMore">查看更多<img src="./img/icon_more.png"/></span>
        </p>
       <div class="recommend-con">
         <div @click="jumpToDetai(1,RcmMine.mineId)" class="zc con-item" style="margin-right: 0.1575rem;" v-for="RcmMine in RcmMineList">
           <h4>{{RcmMine.nameAndVersion}}</h4>
           <p>倒计时{{getCountDownDay(RcmMine.listTime)}}天</p>
           <p>
             <span v-if="RcmMine.coinType">{{getLabel(RcmMine.coinType)}}</span>
           </p>
           <p>
             <span>{{RcmMine.price}}</span>
             <span>
                
             </span>
           </p>
           <p>
             <span>资金成本区间</span>
             <span></span>
           </p>
           <!-- <span class="middleLine"></span> -->
         </div>
       </div>
      </div>
      <div class="hots">
        <div class="hots-head">
          热门资讯
          <span class="label"><img src="./img/label_maintital.png" alt=""></span>
        </div>
        <!--<div class="hots-con">
          <h4 class="hots-con-title">机器人大会来临，强势一触即发！</h4>
          <p class="hots-con-instr">
            <span>未来金融</span>
            <span>新手推荐</span>
            <span>趋势发展</span>
          </p>
          <img class="hots-con-img" src="./img/img3_news.png" alt="">
          <p class="about-read">
            <span class="read-date">2017-12-09</span>
            <span class="read-num">782阅读量</span>
          </p>
        </div>-->
        <div v-for="(news,index) in newsList" v-if="index < 2">
          <hotsItem2 :news="news" @click.native="lookNews(news.link)"></hotsItem2>
        </div>
        <div class="lookMore"  @click="newsDetail">查看更多({{lookCount}})</div>
      </div>
      <div class="suspend" @click="showErweima">
        <img src="./img/half-erwaima.png" alt="">
      </div>
      <!-- <div class="alert-erweima">
        <img src="./img/alert-erweima.png" alt="">
        <img src="./img/close.png" alt="" @click="closeErweima">
      </div> -->

    </div>
    <main-nav which="home"></main-nav>

  </div>
</template>

<script>

import Lib from '@/assets/js/Lib'
import axios from 'axios'

import { Swiper, SwiperItem, } from 'vux'

import MainNav from '@/components/mainNav'

import hotsItem2 from '@/components/hotsItem2'

import $ from "jquery"

export default {
  name: 'Home',	
  components: {
    MainNav, Swiper, SwiperItem,hotsItem2
  },
  data () {
    return {
      RcmMineList:[],
      img_src:[],
      countDownDay1:'',
      countDownDay2:'',
      validPeriod:'',
      newsList:[],
      link:'',
      lookCount:'',
      picLink:''
    }
  },
  mounted(){
    this.getToken();
    this.getWxSig();
    this.showErweima();
    this.closeErweima();
  },
  methods: {
    showErweima(){
      $('.alert-erweima').css('display','block')
    },
    closeErweima(){
      $('.alert-erweima').css('display','none')
    },
    linkTo(picLink){
      if(picLink !== '' && picLink !== null){
        window.location.href = picLink;
      }
    },
    getCountDownDay(listTime){
      return Lib.M.getCountDownDay(listTime);
    },
    //币种类型数字转化为文字
    getLabel(key){
      var f = JSON.parse(localStorage.coinTypeList);
      for(let i in f){
        if(f[i].key == key) return f[i].label
      }
    },
    getToken(){
      var self = this;
      Lib.M.ajax({
        url : Lib.M.apiDomain + '/uaa/oauth/token',
        /*headers: {
          Accept:'application/json',
          Authorization:'Basic Y2xpZW50OnNlY3JldA=='
        },
        params:{
          username:'anil',
          password:'password',
          grant_type:'password',
          scope:'read write'
        },*/
        headers: {
          Accept:'application/json',
          Authorization:'Basic anVoZV9jYXNobG9hbjpKdWhlMTIzNjc4IUAj'
        },
        params:{
          username:'juhe',
          password:'Juhe2017!@#',
          grant_type:'password',
          scope:'read write'
        },
        success:function(data){
          localStorage.token = data.access_token;
          /*self.getOpenId()*/
          /*self.getCarouselFigure();*/
          self.getRecommendZc();
          /*self.getArticle();*/
        },
        error:function(err){
          console.error(err);
        }
      });
    },
    newsDetail(){
      this.$router.push('./NewsDetail')
    },
    jumpToDetai(AorF,proId){
      this.$router.push({'path':'/sqProjectDetail',query:{
          AorF:AorF,
          proId:proId
        }
      })
    },
    lookMore(){
      this.$router.push('./Square')
    },
    getRecommendZc(){
      var self = this;
      Lib.M.ajax({
        url : '/mine/getRecommendMine',
        success:function(res){
          var data = res.data;
          //推两个推荐矿机进RcmList
          var index = parseInt(Math.random()*data.length);
          self.RcmMineList.push(data.splice(index,1)[0]);

          index = parseInt(Math.random()*data.length);
          self.RcmMineList.push(data.splice(index,1)[0]);
          console.log(self.RcmMineList[0])
        },
        error:function(err){
          console.error(err);
        }
      });
    },
    getCarouselFigure(){
      var self = this;
      Lib.M.ajax({
        url:'/info/getCarouselFigure',
        success:function (res) {
          self.img_src=res.data;
        },
        error:function(err){
          console.error(err);
        }
      });
    },
    getArticle(){
      var self = this;
      Lib.M.ajax({
        url:'/info/getArticle',
        success:function (res) {
          self.newsList = res.data;
          self.lookCount = res.data.length - 2;
        },
        error:function(err){
          console.error(err);
        }
      });
    },
    lookNews(link){
       window.location.href = link;
    },
    //获取微信签名
    getWxSig(){
      var self = this;
      Lib.M.ajax({
        url : '/wechat/wxSig',
        data:{url: window.location.href.split('#')[0]},
        success:function(res){
          if(res.code==200){
            let wxSig = res.data;
            Lib.M.setWxShareConfig(wxSig);
          }else{
            self.$vux.toast.text(res.error, 'middle');
          }
        }
      });
    },
  }
}
</script>

<style lang="less" scoped>

  .content{
    .alert-erweima{
      display: none;
      width: 100%;
      height: 100%;
      max-width: 640px;
      max-height: 40rem;
      background: rgba(0,0,0,0.8);
      position: fixed;
      left: 0;
      top: 0;
      img{
        display: block;
        margin: 0 auto;
      }
      img:nth-of-type(1){
        width:15.68rem;
        height:20.63rem;
        margin:5.5rem auto 3rem auto;
      }
      img:nth-of-type(2){
        width:3.32rem;
        height:3.32rem;
      }
    }
    .suspend{
      width: 2.5rem;
      height: 4.78rem;
      position: fixed;
      right: 0;
      bottom: 6rem;
      img{
        width: 100%;
        height: 100%;
      }
    }
    width: 100%;
    background: #EFEFF4;
    padding-bottom:3.8rem;
    position: absolute;
    left: 0;
    top: 0;
    .vux-swiper img{
      width: 100%;
      height: 100%;
    }
    .recommend{
      background: #fff;
      /*height:12rem;*/
      box-sizing: border-box;
      border-top: solid 0.5rem #EFEFF4;
      padding: 0.815rem 0.94rem 1.065rem;
      &>p{
        display:flex;
        justify-content:space-between;
        align-items:center;
        color: #1c3a53;
        &>span:nth-child(1){
          font-size:0.875rem;
        }
        &>span:nth-child(2){
          font-size:0.815rem;
          margin-right: 0.44rem;
          position: relative;
          &>img{
            width:0.22rem;
            height:0.345rem;
            position: absolute;
            top: 0.4rem;
            right: -0.5rem;
          }
        }
      }
      .recommend-con{
        display: flex;
        flex-direction: row;
        .con-item{
          width:0;
          min-height: 7rem;
          flex-grow: 1;
          border: 0.06rem solid #F2F2F2;
          text-align: left;
          padding:0.97rem 0 1.315rem 0.845rem;
          margin:1.065rem 0;
          h4{
            font-size: 0.94rem;
            color: #1C3A53;
            margin-bottom: 0.53rem;
          }
          .middleLine{
            display: block;
            width: 0.06rem;
            height: 1.375rem;
            background: #EAEAEA;
            margin: 1.2rem auto 0;
          }
          p:nth-of-type(1){
            font-size: 0.625rem;
            color: #98A0AF;
          }
          p:nth-of-type(2){
            margin:0.405rem 0 0.905rem 0;
            span{
              width:2.94rem;
              height:0.875rem;
              line-height: 0.875rem;
              text-align: center;
              border: 0.06rem solid #699EFF;
              font-size:0.625rem;
              color:#699EFF;
              margin-right: 0.28rem;
            }
          }
          p:nth-of-type(3){
            position: relative;
            margin-bottom: 0.44rem;
            span{
              font-size:0.875rem;
              color:#F84F60;
              position: absolute;
              font-weight: bolder;
              font-family:PingFangSC-Medium;
            }
            span:nth-of-type(1){
              left:0rem;
              top: 0;
            }
            span:nth-of-type(2){
              right: 1.125rem;
              top: 0;
            }
          }
          p:nth-of-type(4){
            position: relative;
            span{
              font-size:0.625rem;
              color:#98A0AF;
              position: absolute;
            }
            span:nth-of-type(1){
              left: 0rem;
              top: 1.13rem;
            }
            span:nth-of-type(2){
              right: 1.125rem;
              top: 1.13rem;
            }
          }
        }
      }

    }
    .hots{
      margin-top:0.5rem;
      background: #ffffff;
      padding: 0 1rem 1rem;
      .hots-head{
        font-size: 0.875rem;
        color: #1C3A53 ;
        line-height: 3rem;
        border-bottom: 0.06rem solid #EAEAEA;
        text-align: left;
        position: relative;
        .label{
          position: absolute;
          right: 0;
          top: 2.2rem;
          img{
            width: 2.315rem;
            height: 1rem;
          }
        }
      }
      .hots-con{
        margin-top: 1rem;
        text-align: left;
        border-bottom: 0.06rem solid #EAEAEA;
        .hots-con-title{
          font-size: 1rem;
          color: #1C3A53;
        }
        .hots-con-instr{
          margin:0.8rem 0 1rem 0;
          span{
            width: 4.125rem;
            height: 1.315rem;
            padding: 0.5rem;
            background: #F4F5F6;
            font-size: 0.75rem;
            color: #A0ACC0;
            text-align: center;
            line-height: 1.315rem;
          }
        }
        .hots-con-img{
          width: 100%;
          height: 100%;
        }
        .about-read{
          font-size: 0.685rem;
          color: #BFBFBF;
          line-height: 2rem;
          .read-date{
            margin-right: 0.6rem;
          }
        }
      }
      .lookMore{
        width:21.565rem;
        height:2.065rem;
        background:rgba(239,239,244,1);
        border-radius: 0.25rem ;
        margin:1rem auto 0;
        font-size: 0.815rem;
        color: #4083FF;
        line-height: 2.065rem;
        text-align: center;
      }
    }
  }

</style>
