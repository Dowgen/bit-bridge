<template>
  <div>
    <div class="content">
      <!-- <div class="square-head">
        <button-tab class="button-tab">
          <button-tab-item selected @on-item-click="zj=true">资金方</button-tab-item>
          <button-tab-item @on-item-click="zj=false" @click="sortAsset(1)">资产方</button-tab-item>
        </button-tab>
      </div> -->
      <div class="header">
        <p>广场</p>
      </div>
      <div class="zj">
        <div class="zj-head">
       <!-- <div @click="chooseType('.type0')">全部
            <img src="./img/arrow_type_nor.png" alt="" class="arrow-down">
         </div>-->
         <div @click="sortMine(1)" :class="isAsc_time?'active':''">时间
          <img v-show="!isAsc_time" src="./img/ic_arrow_down.png" alt="">
          <img v-show="isAsc_time" src="./img/arrow_up.png" alt="">
         </div>
         <!-- <div @click="isActive_fund='total',isAsc?sort(2,1,2):sort(2,2,2)" :class="isActive_fund=='total'?'active':''">资金规模
           <img v-show="!isAsc" src="./img/ic_arrow_down.png" alt="">
           <img v-show="isAsc" src="./img/arrow_up.png" alt="">
         </div> -->
         <div @click="chooseType('.type1')">币种
            <img src="./img/arrow_type_nor.png" alt="" class="arrow-down">
         </div>
         <div @click="chooseType('.type2')">托管
            <img src="./img/arrow_type_nor.png" alt="" class="arrow-down">
            <!--<img v-show="key == 1" src="./img/arrow_type_sel.png" alt="">-->
         </div>
        </div>
        <!-- <div class="type type0">
          <span class="angle" style="right:19.5rem"><img src="./img/tri-angle.png" alt=""></span>
          <div class="type-head">全部</div>
          <div class="types">
            <label>
              <input v-model="recAuth_fund" type="radio" value="0" />全部
            </label>
            <label>
              <input v-model="recAuth_fund" type="radio" value="1" />推荐项目
            </label>
            <label>
              <input v-model="recAuth_fund" type="radio" value="2" />认证项目
            </label>
          </div>
          <div class="btn">
            <span class="confirm" @click="confirmFund">确定</span>
            <span class="cancel" @click="cancel">取消</span>
          </div>
        </div> -->
        <div class="type type1">
          <span class="angle" style="right:9.5rem"><img src="./img/tri-angle.png" alt=""></span>
          <div class="type-head">币种类型</div>
          <div class="types">
            <label v-for="item in coinTypeList">
              <input v-model="coinType" type="checkbox" :value="item.key" />{{item.label}}
            </label>
          </div>
          <div class="btn">
            <span class="confirm" @click="confirmFund">确定</span>
            <span class="cancel" @click="cancel">取消</span>
          </div>
        </div>

        <div class="type type2">
          <span class="angle"><img src="./img/tri-angle.png" alt=""></span>
          <div class="type-head">托管类型</div>
          <div class="types">
            <label>
              <input v-model="trusteeship" type="checkbox" value="1" />是
              <input v-model="trusteeship" type="checkbox" value="0" />否
            </label>
          </div>
          <div class="btn">
            <span class="confirm" @click="confirmFund">确定</span>
            <span class="cancel" @click="cancel">取消</span>
          </div>
        </div>
        <div class="con">
          <div @click="jumpToDetai(item.mineId)" class="con-item" v-for="(item,index) in mineList">
            <img v-show="item.isAuth==1" src="./img/label_auth.png" class="label-auth">
            <div class="item-title">
              <span></span>
              <span>{{item.nameAndVersion}}</span>
              <!-- <img v-show="item.isRecommend==1" src="./img/label_recommend.png"> -->
              <p>立即购买</p>
            </div>
            <div class="item-main">
              <div class="left">
                <p>{{item.price}}</p> <!--{{item.fundCostRegionFrom}}<span>%</span> - {{item.fundCostRegionTo}}<span>%</span>-->
                <p>商品价格(元)</p>
              </div>
              <div class="right">
                <div>
                  <p></p>
                  <p>币种</p>
                  <p>{{getLabel(item.coinType)}}</p>
                </div>
                <div>
                  <p></p>
                  <p>{{getLabel_arrival(item.arrivalTimeType)}}</p>
                  <p>{{ item.listTime?getCountDownDay(item.listTime):'null' }} <span>天</span></p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- <div class="zj" v-show="!zj">
        <div class="zj-head">
          <div @click="chooseType('.type6')">全部
            <img src="./img/arrow_type_nor.png" alt="" class="arrow-down">
          </div>
          <div @click="sortAsset(1), isActive='comprehension'" :class="isActive=='comprehension'?'active':''">时间
            <img v-show="!isAsc_asset_time" src="./img/ic_arrow_down.png" alt="">
            <img v-show="isAsc_asset_time" src="./img/arrow_up.png" alt="">
          </div>
          <div @click="sortAsset(2), isActive='total'" :class="isActive=='total'?'active':''">规模
            <img v-show="!isAsc_asset_total" src="./img/ic_arrow_down.png" alt="">
            <img v-show="isAsc_asset_total" src="./img/arrow_up.png" alt="">
          </div>
          <div @click="chooseType('.type7')">类型
            <img  src="./img/arrow_type_nor.png" alt="">
           <img v-show="key == 1" src="./img/arrow_type_sel.png" alt="">
          </div>
        </div>
        <div class="type type6">
          <span class="angle" style="right:19.5rem"><img src="./img/tri-angle.png" alt=""></span>
          <div class="type-head">全部</div>
          <div class="types">
            <label>
              <input v-model="recAuth_assest" type="radio" value="0" />全部
            </label>
            <label>
              <input v-model="recAuth_assest" type="radio" value="1" />推荐项目
            </label>
            <label>
              <input v-model="recAuth_assest" type="radio" value="2" />认证项目
            </label>
          </div>
          <div class="btn">
            <span class="confirm" @click="confirmAsset(1)">确定</span>
            <span class="cancel" @click="cancel">取消</span>
          </div>
        </div>
        <div class="type type7">
          <span class="angle"><img src="./img/tri-angle.png" alt=""></span>
          <div class="type-head">产品类型</div>
          <div class="types">
            <label v-for="item in assetTypeList">
              <input v-model="assetType" type="checkbox" :value="item.key" />{{item.label}}
            </label>
          </div>
          <div class="btn">
            <span class="confirm" @click="confirmAsset(1)">确定</span>
            <span class="cancel" @click="cancel">取消</span>
          </div>
        </div>
        <div class="con">
          <div @click="jumpToDetai(1,oItem.assetId)" class="con-item" v-for="oItem in omineList">
          <img v-show="oItem.isAuth==1" src="./img/label_auth.png" class="label-auth">
            <div class="item-title">
              <span></span>
              <span>{{oItem.projectName}}</span>
              <img v-show="oItem.isRecommend==1" src="./img/label_recommend.png">
              <p>立即购买</p>
            </div>
            <div class="item-main">
              <div class="left">
                <p>{{oItem.fundCostRegionFrom}}<span>%</span> - {{oItem.fundCostRegionTo}}<span>%</span></p>
                <p>资金成本区间</p>
              </div>
              <div class="right">
                <div>
                  <p></p>
                  <p>项目类型</p>
                  <p>{{getLabel(oItem.productType,'asset')}}</p>
                </div>
                <div>
                  <p></p>
                  <p>项目倒计时</p>
                  <p>{{ oItem.listTime?getCountDownDay(oItem.listTime):'null' }} <span>天</span></p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div> -->
    </div>
    <main-nav which="square"></main-nav>
  </div>
</template>

<script>

import Lib from '@/assets/js/Lib'

import { ButtonTab, ButtonTabItem } from 'vux'

import MainNav from '@/components/mainNav'

import $ from "jquery"

export default {
  name: 'Square', 
  components: {
    MainNav, ButtonTab, ButtonTabItem
  },
  data () {
    return {
      /*isActive: 'comprehension',*/
      isAsc_time:false,
      key:2,
      mineList:[],
      /*recAuth_fund:'0',*/
      coinTypeList:[],
      coinType:[],
      validPeriod:'',
      countDownDay:'',
      currentTime:'',
      trusteeship:[],
    }
  },
  /*beforeRouteEnter(to, from, next) {
    if(from.path =='/sqProjectDetail'){
      to.meta.keepAlive = true;
    }else{
      to.meta.keepAlive = false;
    }
    next();
  },*/
  beforeRouteLeave(to, from, next) {
    /*if(to.path =='/sqProjectDetail'){
      from.meta.keepAlive = true;
    }else{
      from.meta.keepAlive = false;
    }*/
    from.meta.keepAlive = true;
    next();
  },
  mounted(){
    this.coinTypeList = JSON.parse(localStorage.coinTypeList);
    this.sortMine();
  },
  methods: {
    chooseType(which){
      $(which).css('display','block');
      $(which).siblings(".type").css('display','none');
    },
    /*confirmAsset(chooseType){
      $('.type').css('display','none');
      this.sortAsset(chooseType);
    },*/
    confirmFund(){
      $('.type').css('display','none');
      this.sortMine();
    },
    cancel(){
      $('.type').css('display','none');
    },
    getCountDownDay(beginDate){
      return Lib.M.getCountDownDay(beginDate)
    },
    jumpToDetai(proId){
      this.$router.push({'path':'/sqProjectDetail',query:{
          proId:proId
        }
      })
    },
    /*sortAsset(chooseType){
      var self = this;
      var url = '/asset/sortAsset';
      var sortType = '';
      if(chooseType==1) {
        self.isAsc_asset_time = !self.isAsc_asset_time;
        sortType = self.isAsc_asset_time ? 'asc':'desc';
      }else if(chooseType==2){
        self.isAsc_asset_total = !self.isAsc_asset_total;
        sortType = self.isAsc_asset_total ? 'asc':'desc';
      }

      Lib.M.ajax({
        url:url,
        data:{
          'recAuth':self.recAuth_assest,
          'chooseType':chooseType == 1?'comprehensive':'total',
          'sortType': sortType,
          'type': self.assetType.toString(),
        },
        success:function (res) {
          self.omineList = res.data;
        },
        error:function(err){
          console.error(err);
        }
      });
    },*/
    sortMine(isSort){
      var self = this;
      var url = '/mine/sortMine';
      if(isSort==1) self.isAsc_time = !self.isAsc_time;

      Lib.M.ajax({
        url:url,
        data:{
          /*'recAuth':self.recAuth_fund,*/
          'sortType': self.isAsc_time ?'asc':'desc',
          'coinType': self.coinType.toString(),
          'trusteeship': self.trusteeship.toString(),
        },
        success:function (res) {
           self.mineList = res.data;
        },
        error:function(err){
          console.error(err);
        }
      });
    },
    //币种类型数字转化为文字
    getLabel(key){
      var f = JSON.parse(localStorage.coinTypeList);
      for(let i in f){
        if(f[i].key == parseInt(key)) return f[i].label
      }
    },
    //到货时间类型数字转化为文字
    getLabel_arrival(key){
      var f = JSON.parse(localStorage.arrivalTimeTypeList);
      for(let i in f){
        if(f[i].key == parseInt(key)) return f[i].label
      }
    }
  }
}
</script>

<style lang="less" scoped>
  .content{
    width: 100%;
    min-height: 40rem;
    background: #EFEFF4;
    position: absolute;
    left: 0;
    top: 0;
    padding-bottom: 5rem;
    .active{
      color: #4083ff;
    }
    .header{
      width:100%;
      height:2.75rem;
      background: #fff;
      margin: 0 auto;
      font-size: 1.065rem;
      color: #000;
      text-align: center;
      position: relative;
      line-height: 2.75rem;
      font-weight: 600;
    }
    .square-head{
      width: 100%;
      height: 2.75rem;
      background: #fff;
      overflow: hidden;
      .button-tab{
        width:13.75rem;
        height: 1.6rem;
        margin:0.575rem auto 0;
        border-radius: 4px;
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
    }
    .zj{
      .zj-head{
        display: flex;
        flex-direction: row;
        margin-top: 0.8rem;
        color: #a0acc0;
        div{
          flex: 1;
        }
        div:nth-of-type(2){
          position: relative;
          img{
            width: 0.315rem;
            height: 0.25rem;
            position: absolute;
            top: 0.6rem;
            right:1.15rem;
          }
        }
        div:nth-of-type(3){
          position: relative;
          img{
            width: 0.315rem;
            height: 0.565rem;
            position: absolute;
            top: 0.4rem;
            right:0.65rem;
          }
          .arrow-down{
            width: 0.315rem;
            height: 0.25rem;
            position: absolute;
            top: 0.6rem;
            right: 1.2rem;
          }
        }
        div:nth-of-type(1),div:nth-of-type(4){
          position: relative;
          img{
            width: 0.315rem;
            height: 0.5rem;
            position: absolute;
            top: 0.5rem;
            right: 1.2rem;
          }
        }
      }
      .con{
        .con-item{
          height: 7.5rem;
          background-color: #fff;
          margin-top: 0.8rem;
          padding: 0 1rem;
          position: relative;
          .item-title{
            display:flex;
            align-mineList:center;
            text-align: left;
            line-height: 2.315rem;
            font-size: 0.875rem;
            color: #333333 ;
            border-bottom:0.06rem dashed #EAEAEA;
            position: relative;
            span:nth-of-type(1){
              display: inline-block;
              width: 0.2rem;
              height: 0.9rem;
              background: #4083FF;
              position: absolute;
              top: 0.65rem;
              margin-right: 1rem;
            }
            span:nth-of-type(2){
              margin-left: 0.5rem;
            }
            img{
              width:2.25rem;
              height:1rem;
              margin-left:0.625rem;
            }
            p{
              font-size:0.5rem;
              font-weight:300;
              letter-spacing:1px;
              position:absolute;
              width:4rem;
              height:1.5rem;
              line-height:1.5rem;
              text-align:center;
              right:0.6rem;
              top:0.6rem;
              background-color:#fc4a1a;
              color:white;
              border-radius:3px;
            }
          }
          .item-main{
            display: flex;
            flex-direction: row;
            .left{
              flex: 1;
              text-align: left;
              p:nth-of-type(1){
                font-size: 1.5rem;
                color: #FF671B;
                margin:1.25rem 0 0.4rem;
                span{
                  font-size: 0.815rem;
                }
              }
              P:nth-of-type(2){
                font-size: 0.75rem;
                color: #A3A3A3;
              }
            }
            .right{
              flex: 2;
              height: 3rem;
              border-left: 0.06rem solid #E6E7EB;
              margin-top: 1.47rem;
              div{
                font-size: 0.75rem;
                overflow: hidden;
                position: relative;
                p:nth-of-type(1){
                  display: inline-block;
                  width: 0.32rem;
                  height: 0.32rem;
                  border-radius: 50%;
                  background: #F3AE43;
                  float: left;
                  position: absolute;
                  left: 4.56rem;
                  top: 0.3rem;
                }
                p:nth-of-type(2){
                  display: inline-block;
                  color: #A3A3A3;
                  float: left;
                  margin-left: 5.315rem;
                  margin-bottom: 0.75rem;
                }
                p:nth-of-type(3){
                  display: inline-block;
                  color: #4083FF;
                  float: right;
                  span{
                    color: #666;
                  }
                }
              }

            }
          }
        }

      }
      .type{
        height: 33rem;
        background: #fff;
        text-align: left;
        margin-top: 1rem;
        position: relative;
        display: none;
        .angle{
          display: block;
          width: 2rem;
          height: 2rem;
          position: absolute;
          right: 1.9rem;
          top: -1.2rem;
          img{
            width: 100%;
            height: 100%;
          }
        }
        .type-head{
          font-size: 0.94rem;
          color: #1A1A1A;
          height: 2.8rem;
          line-height: 2.8rem;
          border-bottom:0.06rem solid #EAEAEA;
          text-indent: 1rem;
        }
        .types{
          padding: 0 1rem;
          text-align: center;
          margin-top: 1rem;
          input[type="checkbox"],input[type="radio"] {
            display: inline-block;
            width: 1.19rem;
            height: 1.19rem;
            border: 0.06rem solid #E6E6E6;
            border-radius: 0.19rem;
            cursor: pointer;
            margin-right: 0.2rem;
            outline: none;
          }
          input:checked{
            background: url("./img/ic_selected_more.png") no-repeat center;
            background-size: 100% 100%;
          }
          label {
            display: inline-block;
            height: 3rem;
            line-height: 3rem;
            margin-right: 0.5rem;
          }
        }
        .btn{
          text-align: center;
          margin-top: 3rem;
          span{
            display: inline-block;
            width:8.315rem;
            height:2.815rem;
            background:rgba(64,131,255,1);
            border-radius: 0.315rem ;
            font-size: 1.125rem;
            text-align: center;
            line-height: 2.815rem;
          }
          span:nth-of-type(1){
            color: #FFFFFF;
            margin-right: 1.28rem;
          }
          span:nth-of-type(2){
            color: #B5B5B5;
            background: #ffff;
            border:0.06rem solid #EAEAEA;
          }
        }
      }

    }
    .label-auth{
      width:6.22rem;
      height:5.5rem;
      position:absolute;
      right:1%;
      bottom:5%;
    }
    
  }

</style>
