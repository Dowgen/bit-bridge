<template>
  <div v-cloak class="content">
    <myHead v-show="false" msg="发布项目" backgroundColor="#fff" hasBack="0"></myHead>
    <actionsheet v-model="show1" :menus="menus1" @on-click-menu="jumpToRelease"></actionsheet>
    <div v-if="hasProject=='no'" class="content1">
      <button-tab class="button-tab">
        <button-tab-item :selected="loseEfficacy=='0'" @on-item-click="loseEfficacy='0'">未失效</button-tab-item>
        <button-tab-item :selected="loseEfficacy=='1'" @on-item-click="loseEfficacy='1'">已失效</button-tab-item>
      </button-tab>
      <div class="btn addAs" @click="jumpToRelease">
        <span>+</span> 添加矿机
      </div>
      <!-- <div class="btn addFu" @click="jumpToRelease('addFund')">
        <span>+</span> 添加资金
      </div> -->
    </div>
    <div v-if="hasProject=='yes'" class="content2">
      <button-tab class="button-tab">
        <button-tab-item :selected="loseEfficacy=='0'" @on-item-click="loseEfficacy='0'">未失效</button-tab-item>
        <button-tab-item :selected="loseEfficacy=='1'" @on-item-click="loseEfficacy='1'">已失效</button-tab-item>
      </button-tab>
      <div class="using" v-show="loseEfficacy=='0'">
      <div class="fund-item-con">
        <!-- <div @click="show1=true" class="item_add"><img src="./img/item_add.png"></div> -->
        <!-- <popover placement="bottom">
          <div class="item_add"><img src="./img/item_add.png"></div>
          <div slot="content" class="popover-demo-content">
            <span>+</span> 添加资金
          </div>
        </popover> -->
        <!-- <h3>资金</h3>
        <p>合规对接 量大稳定</p> -->
        <div>
        <div @click="jumpToDetail(item.mineId)" class="item" v-for="item in mineList">
          <div class="title">
            <span>{{item.nameAndVersion}}</span>
            <span>{{item.arrivalTime}} 发货</span>
          </div>
          <div class="item-con">
            <div>
              <p>{{item.price}}</p>
              <span>商品价格(元)</span>
            </div>
            <div>
              <p>{{getLabel(item.coinType)}}</p>
              <span>币种</span>
            </div>
            <div class="label">
              <img v-show="item.listStatus==1" src="./img/label_judge.png"/>
              <img v-show="item.listStatus==2" src="./img/label_online.png"/>
            </div>
          </div>
        </div>
        </div>
        <!-- <div class="seeAll" @click="showItem02=fundList.length">
          查看全部({{fundList.length}})
        </div> -->
      </div>
      <!-- <div class="assets-item-con">
        <h3>资产</h3>
        <p>风控审核 多元供给</p>
        <div>
        <div class="item" v-for="item in mineList.slice(0,showItem01)" @click="jumpTo(1,item.assetId,'0')">
          <div class="left">
            <p>{{item.fundCostRegionFrom}}<span>%</span> 
            - {{item.fundCostRegionTo}}<span>%</span></p>
            <span>资金成本区间</span>
          </div>
          <div class="right">
            <p>{{item.projectName}}</p>
            <div>
              <div>
                <span v-if="item.listStatus==2">
                {{getCountDownDay(item.listTime,validPeriod)}}天</span>
                <i v-if="item.listStatus==2">|</i>
                <span>{{getLabel(item.productType,'asset')}}</span>
              </div>
              <img v-show="item.listStatus==1" src="./img/label_judge.png"/>
              <img v-show="item.listStatus==2" src="./img/label_online.png"/>
            </div>
          </div>
        </div>
        </div>
        <div class="seeAll" @click="showItem01=mineList.length">
          查看全部({{mineList.length}})
        </div>
      </div> -->
      </div>

      <div class="useless" v-show="loseEfficacy=='1'">
      <div class="fund-item-con">
        <!-- <h3>资金</h3>
        <p>合规对接 量大稳定</p> -->
        <div>
        <div @click="jumpToDetail(item.mineId)" class="item" v-for="item in mineListLose">

          <img class="img-unpassed" v-show="item.listStatus==3" src="./img/label_unpassed.png"/>

          <div class="title">
            <span>{{item.nameAndVersion}}</span>
            <span>{{item.arrivalTime}} 发货</span>
          </div>
          <div class="item-con">
            <div>
              <p>{{item.price}}</p>
              <span>商品价格(元)</span>
            </div>
            <div>
              <p>{{getLabel(item.coinType)}}</p>
              <span>币种</span>
            </div>
            <div class="label">
              <img v-show="item.listStatus==4" src="./img/label_timeup.png"/>
            </div>
          </div>
        </div>
        </div>
        <!-- <div class="seeAll" @click="showItem12=fundListLose.length">
          查看全部({{fundListLose.length}})
        </div> -->
      </div>
      <!-- <div class="assets-item-con">
        <h3>资产</h3>
        <p>风控审核 多元供给</p>
        <div>
        <div class="item" v-for="item in mineListLose.slice(0,showItem11)" @click="jumpTo(1,item.assetId,'1')">
          <img class="img-unpassed" v-show="item.listStatus==3" src="./img/label_unpassed.png"/>
      
          <div class="left">
            <p>{{item.fundCostRegionFrom}}<span>%</span> 
            - {{item.fundCostRegionTo}}<span>%</span></p>
            <span>资金成本区间</span>
          </div>
          <div class="right">
            <p>{{item.projectName}}</p>
            <div>
              <div>
                <span v-if="item.listStatus==2">
                {{getCountDownDay(item.listTime,validPeriod)}}天</span>
                <i v-if="item.listStatus==2">|</i>
                <span>{{getLabel(item.productType,'asset')}}</span>
              </div>
              <img v-show="item.listStatus==4" src="./img/label_timeup.png"/>
            </div>
          </div>
        </div>
        </div>
        <div class="seeAll" @click="showItem11=mineListLose.length">
          查看全部({{mineListLose.length}})
        </div>
      </div> -->
      </div>

      <div class="item-add">
        <img v-show="show1==false" src="./img/add_button_nor.png" @click="show1=true">
        <img v-show="show1==true" src="./img/add_button_press.png">
      </div>
    </div>
    <main-nav which="MyProject"></main-nav>
  </div>
</template>

<script>

import Lib from '@/assets/js/Lib'
import myHead from '@/components/myHead'
import MainNav from '@/components/mainNav'
import { ButtonTab, ButtonTabItem,Actionsheet } from 'vux'


export default {
  name: 'Home', 
  components: {
    myHead, MainNav, ButtonTab, ButtonTabItem, Actionsheet
  },
  data () {
    return {
      showItem01:3,//未失效，资产
      showItem11:3,//已失效，资产
      showItem02:3,//未失效，资金
      showItem12:3,//已失效，资金
      hasProject:null,
      loseEfficacy:'0',
      validPeriod:null,
      mineList:[],
      mineListLose:[],
      /* actionsheet */
      show1: false,
      /*menus1: {
        addFund: '添加资金',
        addAssets: '添加资产'
      },*/
      menus1:{
        addMachine:'添加矿机'
      }
    }
  },
  beforeRouteLeave(to, from, next) {
    /*if(to.path =='/sqProjectDetail'){
      from.meta.keepAlive = true;
    }else{
      from.meta.keepAlive = false;
    }*/
    console.log(this.loseEfficacy);
    sessionStorage.loseEfficacy = this.loseEfficacy;
    next();
  },
  mounted(){
    if(sessionStorage.loseEfficacy != null){
      this.loseEfficacy = sessionStorage.loseEfficacy;
      console.log(this.loseEfficacy)
    }

    this.getMyProject();
  },
  methods: {
    //币种类型数字转化为文字
    getLabel(key){
      var f = JSON.parse(localStorage.coinTypeList);
      for(let i in f){
        if(f[i].key == key) return f[i].label
      }
    },
    jumpToRelease (key) {
      this.$router.push('/Release1')
    },
    /*getConfigByParameter(){
      var self = this;
      Lib.M.ajax({
        url:'/config/getConfigByParameter',
        data:{
          'key':'unlistPeriod'
        },
        success:function (res) {
          self.validPeriod = res.data[0].value;
          console.log(111111);
          console.log(self.validPeriod);
        },
        error:function(err){
          console.error(err);
        }
      });
    },*/
    /* 查询个人项目 */
    getMyProject(){
      var self = this;
      Lib.M.ajax({
        url : '/mine/findMineByUserId',
        data: { userId: localStorage.userId },
        success:function(res){
          let data = res.data;
          if(res.code==200){
            if(data.length == 0){
              self.hasProject = 'no';
            }else{
              self.hasProject = 'yes';
              //区分未失效列表与已失效列表
              for(let i in data){
                if(data[i].listStatus == 1 || data[i].listStatus == 2){
                  self.mineList.push(data[i]);
                }else{
                  self.mineListLose.push(data[i]);
                }
              }
            }
          }else{
            self.$vux.toast.text(res.error, 'middle')
          }
        }
      });
    },
    //跳转至详情页
    jumpToDetail(proId){
      var self = this;
      this.$router.push(
        { 
          path:'/ProjectDetail',
          query:{
            proId:proId,
            isLose: self.loseEfficacy
          }
        }
      )
    },
    //倒计时计算
    getCountDownDay(beginDate,efDay){
      return Lib.M.getCountDownDay(beginDate,efDay);
    }
  }
}
</script>

<style lang="less" scoped>
  .content{
    background: #fff;
  }

  .img-unpassed{
    width: 4.53rem;
    height: 3.7rem;
    position: absolute;
    right: 0;
    bottom: 1rem;
  }
  .item-add{
    position:fixed;
    width: 5rem;
    height: 4.815rem;
    z-index:10;
    bottom:2.7rem; right:0;
    margin:auto;
    img{
      width: 5rem;
      height: 4.815rem;
    }
  }
  .content1{
    box-sizing:border-box;
    /* padding-top: 1rem; */
    background:url('./img/empty_page.png') no-repeat center;
    background-position:center 7.5rem;
    background-size: 12.22rem 13.625rem;
    height:40rem;
    .btn{
      width:11.28rem;
      height: 2.815rem;
      border-radius:1.405rem;
      font-size: 1.065rem;
      display:flex;
      align-items:center;
      justify-content:center;
      margin:0 auto;
      span{
        font-weight:600;
        font-size: 1.8rem;
        margin-right:0.4rem;
      }
    }
    .addAs{
      margin-top:22rem;
      background-color:#4083FF;
      color:#fff;
    }
    .addFu{
      background-color:#fff;
      color:#4083FF;
      border:solid 1px #4083FF;
      margin-top:1rem;
    }
  }
  .content2{
    box-sizing:border-box;
    /* padding-top: 1rem; */
    padding-bottom: 4.3rem;
  }
  .button-tab{
    width:13.75rem;
    height: 1.6rem;
    margin:1rem auto;
  }
  .fund-item-con,.assets-item-con{
    position:relative;
    box-sizing:border-box;
    /*padding: 1.44rem 0.97rem 0;*/
    .item_add{
      width:4rem;
      height:2rem;
      position:absolute;
      right:0.97rem;
      top:1.84rem;
      img{
        width:1.72rem;
        height:0.315rem;
      }
    }
    >h3,>p{text-align:left;}
    >h3{
      color:#333;
      font-size:1.125rem;
    }
    >p{
      color: #a0acc0;
      font-size: 0.815rem;
      margin-top: 0.1rem;
      margin-bottom: 1rem;
    }
    .item{
      background: #fff;
      border-top: 0.5rem solid #EFEFF4;
      position:relative;
      padding:0 1rem;
      /*display:flex;
      justify-content:space-between;*/
     /* border-bottom:dashed 2px #e5e5e5;*/
      &:last-child{border-bottom:solid 2px #fff;
    }
    .title{
      height: 2.125rem;
      line-height: 2.125rem;
      border-bottom:solid 1px #e5e5e5;
    }
    .title span:nth-of-type(1){
      font-size: 0.875rem;
      color: #1A1A1A;
      float: left;
    }
    .title span:nth-of-type(2){
      font-size: 0.815rem;
      color: #4083FF;
      float: right;
    }
    .item-con{
      display: flex;
    }
    .item-con div:nth-of-type(1){
      flex: 1;
      text-align: left;
    }
    .item-con div:nth-of-type(1) p{
        font-size: 1.19rem;
        color: #FC743F;
        margin:1.35rem 0 0.9rem 0;
    }
    .item-con div:nth-of-type(1) span{
      font-size: 0.75rem;
      color: #A3A3A3;
      margin-bottom: 0.94rem;
      display: inline-block;
    }
    .item-con div:nth-of-type(2){
      flex: 1;
      text-align: center;
    }
    .item-con div:nth-of-type(2) p{
      font-size: 0.875rem;
      color: #000;
      margin:1.35rem 0 1.19rem 0;
    }
    .item-con div:nth-of-type(2) span{
      font-size: 0.75rem;
      color: #A3A3A3;
      margin-bottom: 0.94rem;
      display: inline-block;
    }
    .item-con div:nth-of-type(3){
      flex: 1;
      text-align: right;
    }
     /* >div{
        display:flex;
        flex-direction:column;
        justify-content:space-between;
      }
      >.left{
        >p{
          font-size:1.5rem;
          color: #fc743f;
          >span{
            font-size:0.815rem;
          }
        }
        >span{
          font-size:0.875rem;
          color: #586575;
          margin-top:1.22rem;
        }
      }
      >.right{
        margin-right:1rem;
        width:10.5rem;
        text-align: left;
        >p{
          font-size: 1rem;
          color: #1c3a53;
        }
        >div{
          margin-top: 1.22rem;
          color:#586575;
          font-size:0.875rem;
          display:flex;
          align-items:center;
          justify-content:space-between;
          >div{
            flex:4;
            >i{
              margin:0 0.3rem;
            }
          }
          img{
            flex:1;
            width:3rem;
            height: 1.15rem;
          }
        }
      }*/
    }
    .seeAll{
      width:21.565rem;
      height: 2.065rem;
      line-height: 2.065rem;
      background-color:#efeff4;
      color: #4083FF;
      font-size: 0.815rem;
      margin:0 auto;
      margin-top:0.1rem;
    }
  }
  .label{
    display:flex;
    justify-content:flex-end;
    align-items:center;
    img{
      width:3rem;
      height:1.15rem;
    }
  }
  .useless{
    .title>span:nth-of-type(1){
      color:#5c7d99 !important;
    }
    .title>span:nth-of-type(2){
      color:#a3a3a3 !important;
    }
    .item-con>div>p{
      color:#adb4b8 !important;
    }
  }
  .vux-button-group > a.vux-button-tab-item-first {
    border-top-left-radius: 4px;
    border-bottom-left-radius: 4px;
  }
  .vux-button-group > a.vux-button-tab-item-last {
    border-top-right-radius: 4px;
    border-bottom-right-radius: 4px;
    border-color:#4083ff;
  }
  .vux-button-group > a.vux-button-tab-item-first:after{
    border-top-left-radius: 4px;
    border-bottom-left-radius: 4px;
  }
  .vux-button-group > a.vux-button-tab-item-last:after{
    border-top-right-radius: 4px;
    border-bottom-right-radius: 4px;
  }
</style>
