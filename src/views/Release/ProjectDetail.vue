<template>
  <div class="project">
    <!-- <div v-if="$route.query.AorF==1"> -->
    <div class="project-head">
      <myHead :msg="'项目详情'" backgroundColor="#fff"></myHead>
      <div class="con">
        <p class="p1"><span class="p1-line"></span>{{info.nameAndVersion}}</p>
        <p class="p3">
          <span class="p3-line-l"></span>
          商品价格(元)
          <span class="p3-line-r"></span>
        </p>
        <p class="p2">{{info.price}}</p>
        <div class="project-des">
          <div class="item">
            <p>{{info.consume}}</p>
            <p>消耗功率</p>
          </div>
          <div class="item">
            <p>{{info.saleNum}}</p>
            <p style="border: none;">起售数量</p>
          </div>
        </div>
      </div>
    </div>
    <div class="project-main">
      <div class="main-item">
        <div>
          <p>币种</p>
          <p>{{getLabel(info.nameAndVersion)}}</p>
        </div>
        <div>
          <p>到货时间</p>
          <p>{{info.arrivalTime}}</p><!---->
        </div>
        <div>
          <p>产品状态</p>
          <p>
           {{info.productStatus}}
          </p>
        </div>
        <div>
          <p>算力</p>
          <p>{{info.calculateStress}}</p>
        </div>
        <div>
          <p>是否托管</p>
          <p>{{convert(info.trusteeship)}}</p>
        </div>
        <div>
          <p>产品介绍</p>
          <p>{{info.productIntroduction}}</p>
        </div>
      </div>
    </div>
    <div class="project-main project-footer">
      <div class="main-item">
        <div>
          <p>售后</p>
          <p style="word-wrap: break-word; word-break: break-all;">{{info.customerService}}</p>
        </div>
        <div>
          <p>是否配有官方电源</p>
          <p>{{convert(info.powerSupply)}}</p>
        </div>
        <!-- <div>
          <p>资金来源</p>
          <p>{{getLabel(info.fundOrigin,'assest')}}</p>
        </div>
        <div>
          <p>公司地址</p>
          <p>{{info.companyAddress}}</p>
        </div>
        <div>
          <p>公司背景</p>
          <p>{{info.companyBackground || '无'}}</p>
        </div> -->
      </div>
    </div>
    <div class="project-main project-footer">
      <div class="main-item">
        <div>
          <p>手机号码</p>
          <p>{{info.contactPhone}}</p>
        </div>
        <div>
          <p>微信号</p>
          <p>{{info.contactWechat}}</p>
        </div>
        <div>
          <p>QQ号</p>
          <p>{{info.contactQQ}}</p>
        </div>
      </div>
    </div>
    <div class="footer-btn" style="display: flex;flex-direction: row">
      <div v-show="isLose == '0' && info.listStatus == 2" class="btn-right" style="flex-grow: 1" @click="click('offline')">下架</div>
      <!-- <div v-show="isLose == '1'&& info.listStatus != 3" class="btn-left" style="flex-grow: 1" @click="shareTip">分享</div> -->
      <div v-show="isLose == '1'" class="btn-right" style="flex-grow: 1" @click="click('delete')">
        <img src="./img/delete.png" alt="" class="delete">
        删除
      </div>
    </div>
  </div>

  <!-- <div class="project" v-else-if="$route.query.AorF==2">
    <div class="project-head">
      <div class="head">
        <span @click="$router.go(-1)" class="back"><img style=" width:0.72rem;height: 1.22rem;display: inline-block;" src="./img/back.png" alt=""></span>
        <p>项目详情</p>
      </div>
      <div class="con">
        <p class="p1">{{info.projectName}}</p>
        <p class="p2">{{info.fundCostRegionFrom}}-{{info.fundCostRegionTo}}</p>
        <p class="p3">资金成本区间(%）</p>
        <div class="project-des">
          <div class="item">
            <p>资金类型: {{getLabel(info.fundType,'fund')}}</p>
          </div>
          <div class="item">
            <p>资金规模: 
              {{ String(parseInt(info.fundAnmount)).length >= 5 ? info.fundAnmount/10000 : info.fundAnmount}} 
              {{ String(parseInt(info.fundAnmount)).length >= 5 ? '亿元' : '万元'}}
            </p>
            <p>资金规模:{{getFundAmountType(info.fundAnmount)}}</p>
          </div>
        </div>
      </div>
    </div>
    <div class="project-main">
      <div class="main-item">
        <div>
          <p>公司名称</p>
          <p>{{info.companyName}}</p>
        </div>
        <div>
          <p>青睐资产</p>
          <p>{{getLabel(info.findAssetType,'asset')}}</p>
        </div>
        <div>
          <p>资金需求</p>
          <p>{{info.findAssetRequire}}</p>
        </div>
        <div>
          <p>备注</p>
          <p>{{info.remark}}</p>
        </div>
      </div>
    </div>
    <div class="project-main project-footer">
      <div class="main-item">
        <div>
          <p>手机号码</p>
          <p>{{info.contactPhone}}</p>
        </div>
        <div>
          <p>微信号</p>
          <p>{{info.contactWechat}}</p>
        </div>
        <p>QQ号</p>
        <p>{{info.contactQQ}}</p>
      </div>
    </div>
    <div class="footer-btn" style="display: flex;flex-direction: row">
      <div v-show="isLose == '0' && info.listStatus == 2" class="btn-left" style="flex-grow: 1" @click="click('offline')">下架</div>
      <div v-show="isLose == '1'&&info.listStatus != 3" class="btn-left" style="flex-grow: 1" @click="shareTip">分享</div>
      <div v-show="isLose == '1'" class="btn-right" style="flex-grow: 1" @click="click('delete')">
        <img src="./img/delete.png" alt="" class="delete">
        删除
      </div>
    </div>
  </div> -->
</template>

<script>
import Lib from '@/assets/js/Lib'
import myHead from '@/components/myHead'
import { Toast,Loading } from 'vux'
import axios from 'axios'

export default {
  name: 'ProjectDetail',
  components: {
    Toast,Loading,myHead
  },
  data () {
    return {
      AorF:null,
      isLose:null,
      info:{},
      wxSig:{},
      wechatShareReturnLink:''
    }
  },
  mounted(){
    this.AorF = this.$route.query.AorF;
    this.isLose = this.$route.query.isLose;
    this.getDetail();
    /*this.getReturnLink();*/
  },
  methods:{
    /*getReturnLink(){
      Lib.M.ajax({
        url : '/config/getConfigByParameter',
        data: {
          key: 'wechatShareReturnLink'
        },
        success:function(res){
          if(res.code==200){
            self.wechatShareReturnLink = res.data[0].value;
          }else{
            self.$vux.toast.text(res.error, 'middle');
          }
        }
      });
    },*/
    /*getFundAmountType(key){
      return Lib.M.getFundAmountType(key);
    },*/
    shareSuccess(){
      this.$vux.toast.show({
        showPositionValue: false,
        text: '分享成功',
        type: 'success',
        position: 'middle'
      })
    },
    shareTip(){
      this.$vux.alert.show('微信分享项目即可重新生效')
    },
    click(fun){
      var self = this, content = null, func=null;
      if(fun=='offline'){
        content = '下架之后，该项目将进入已失效，是否确认下架？';
        func = self.offline;
      }else if(fun=='delete'){
        content = '是否确认删除此项目？';
        func = self.delete;
      }
      this.$vux.confirm.show({
        content: content,
        onConfirm () {
          func();
        }
      })
    },
    //下架
    offline(){
      var self = this;
      Lib.M.ajax({
        url : '/mine/unListMine',
        data:{unListId:self.info.mineId},
        success:function(res){
          if(res.code==200){
            self.$vux.toast.text('下架成功!', 'middle');
            self.$router.replace('MyProject');
          }else{
            self.$vux.toast.text(res.error, 'middle');
          }
        }
      });
    },
    //删除
    delete(){
      var self = this;
      Lib.M.ajax({
        url : '/mine/deleteOnListMine',
        data:{deleteId:self.info.mineId},
        success:function(res){
          if(res.code==200){
            self.$vux.toast.text('删除成功!', 'middle');
            self.$router.replace('MyProject');
          }else{
            self.$vux.toast.text(res.error, 'middle');
          }
        }
      });
    }, 
    //分享
    /*share(){
      var self = this;
      Lib.M.ajax({
        url : '/public/reListProject',
        data:{reListId:self.AorF==1?self.info.assetId:self.info.fundId},
        success:function(res){
          if(res.code==200){
            self.shareSuccess();
          }else{
            self.$vux.toast.text(res.error, 'middle');
          }
        }
      });

    },*/
    //获取微信签名
    getWxSig(){
      var self = this;
      Lib.M.ajax({
        url : '/wechat/wxSig',
        data:{url: location.href.split('#')[0]},
        success:function(res){
          if(res.code==200){
            let wxSig = res.data;
            wx.config({
              debug: false, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
              appId: wxSig.appid, // 必填，公众号的唯一标识
              timestamp: wxSig.timestamp, // 必填，生成签名的时间戳
              nonceStr:  wxSig.noncestr, // 必填，生成签名的随机串
              signature: wxSig.signature,   // 必填，签名，见附录1
              jsApiList: ["onMenuShareTimeline","onMenuShareAppMessage"] // 必填，需要使用的JS接口列表，所有JS接口列表见附录2
            });
            //微信分享设置
            wx.onMenuShareTimeline({
              title: self.info.projectName, 
              link: Lib.M.webDomain + '?from=singlemessage' + '/#/sqProjectDetail?&proId=' + self.$route.query.proId
                + '&fromShare=y', 
              imgUrl: Lib.M.webDomain+'/logo.png', 
              success: function () { 
                /*if(self.isLose=='1' && self.info.listStatus== 4) self.share();*/
                self.shareSuccess();
              },
              cancel: function () { 
                  // 用户取消分享后执行的回调函数
              }
            });

            wx.onMenuShareAppMessage({
              title: self.info.projectName, 
              desc: '关注51资金资产公众号，获取更多信息', 
              link: Lib.M.webDomain + '?from=singlemessage' + '/#/sqProjectDetail?&proId=' + self.$route.query.proId
                + '&fromShare=y',
              imgUrl: Lib.M.webDomain+'/logo.png', 
              /*type: '', // 分享类型,music、video或link，不填默认为link*/
              /*dataUrl: '', // 如果type是music或video，则要提供数据链接，默认为空*/
              success: function () { 
                /*if(self.isLose=='1' && self.info.listStatus==4) self.share();*/
                self.shareSuccess();
              },
              cancel: function () { 
                  // 用户取消分享后执行的回调函数
              }
            });
          }else{
            self.$vux.toast.text(res.error, 'middle');
          }
        }
      });
    },
    //根据id查询详情
    getDetail(){
      var self = this;
      Lib.M.ajax({
        url :'/mine/findMineByMineId',
        data: {mineId:self.$route.query.proId},
        success:function(res){
          if(res.code==200){
            self.info = res.data;
            self.getWxSig();
          }else{
            self.$vux.toast.text(res.error, 'middle');
          }
        }
      });
    },
    //币种类型数字转化为文字
    getLabel(key){
      var f = JSON.parse(localStorage.coinTypeList);
      for(let i in f){
        if(f[i].key == key) return f[i].label
      }
    },
    //1是0否转换
    convert(key){
      if(key == '1') return '是'
      else return '否'
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
body{
  padding-bottom:0 !important;
}
.p1-line{
  display: inline-block;
  width: 0.125rem;
  height: 0.875rem;
  background: #4083FF;
  margin-right: 0.5rem;
}
.p3-line-l{
  display: inline-block;
  width: 1rem;
  height: 0.065rem;
  background: #999;
  position: absolute;
  left: 8rem;
  top: 0.5rem;
}
.p3-line-r{
  display: inline-block;
  width: 1rem;
  height: 0.065rem;
  background: #999;
  position: absolute;
  right: 8rem;
  top: 0.5rem;
}


.alert-erweima{
  display: none;
  width: 100%;
  height: 100%;
  max-width: 640px;
  min-height: 40rem;
  background: rgba(0,0,0,0.8);
  position: fixed;
  left: 0;
  top: 0;
}
.alert-erweima img{
  display: block;
  margin: 0 auto;
}
.alert-erweima img:nth-of-type(1){
  width:15.68rem;
  height:20.63rem;
  margin:8rem auto 3rem auto;
}
.alert-erweima img:nth-of-type(2){
  width:3.32rem;
  height:3.32rem;
}
.suspend{
  width: 2.5rem;
  height: 4.78rem;
  position: fixed;
  right: 0;
  bottom: 6rem;
}
.suspend img{
  width: 100%;
  height: 100%;
}
.project{
  box-sizing:border-box;
  width: 100%;
  min-height: 40rem;
  background: #fff;
  padding-bottom: 3.065rem;
}
.project-head{
  width: 100%;
  height: 15.69rem;
  background: #fff;
  background-size: 100% 100%;
  overflow: hidden;
  color:#333;
}
.head{
  width: 100%;
  height: 1.22rem;
  color: #fff;
  position: relative;
  margin-top: 1.3rem;
}
.head p{
  font-size:1.065rem;
  text-align: center;
}
.head span{
  position: absolute;
  left: 1.655rem;
}
.con p{
  color: #fff;
}
.con .p1{
  font-size: 0.94rem;
  margin:1rem 0 1.6rem 1.625rem;
  color: #333;
  text-align: left;
}
.con .p2{
  font-size: 1.5rem;
  color: #666;
}
.con .p3{
  font-size: 0.75rem;
  color: #666;
  margin-bottom: 1rem;
  position: relative;
}
.project-des{
  display: flex;
  flex-direction: row;
  margin-top: 1.5rem;
}
.project-des .item{
  flex-grow: 1;
  width: 0;
}
.project-des .item p:nth-of-type(1){
  font-size: 0.75rem;
  color: #ababab;
  border-right: 1px solid #fff;
}
.project-des .item p:nth-of-type(2){
  font-size: 0.875rem;
  color:#333;
}
.project-main{
  width: 100%;
  margin: 0 auto;
  background: #fff;
  border-top: solid 0.625rem #d7d7d7;
}
.project-main .main-item{
  font-size: 0.875rem;
  text-align: left;
  align-self:baseline;
  padding:0.75rem 1rem;
}
.project-main .main-item>div{
  display: flex;
}
.project-main .main-item p{
  margin:0.75rem 0;
}
.project-main .main-item>div>p:nth-of-type(1){
  color: #BEBEBE;
  flex:1;
}
.project-main .main-item>div>p:nth-of-type(2){
  color: #666666;
  flex:3;
  margin-left: 2rem;
}
.project-footer{
  border-top: solid 0.625rem #d7d7d7;
}
.footer-btn{
  display: flex;
  flex-direction: row;
  position: fixed;
  bottom: 0;
  width: 100%;
  max-width: 640px;
  height: 3.065rem;
  color: #fff;
  text-align: center;
  line-height: 3.065rem;
  font-size: 1.065rem;
}
.btn-right{
  flex:30;
  background: #4083FF ;
  font-size: 1.125rem;
  color: #fff;
}
.btn-left{
  background: url('./img/button_bg.png');
  background-size: 100% 100%;
  flex:16;
  font-size: 1.125rem;
  color: #B5B5B5;
}
.alert{
  width: 100%;
  height: 100%;
  max-width: 640px;
  position: absolute;
  left: 0;
  top: 0;
  background: rgba(0,0,0,0.45);
  display: none;
}
.alert .contactCard{
  width: 14.815rem;
  height: 21.5rem;
  background: url("./img/contact_card_bg.png") no-repeat center;
  background-size: 100% 100%;
  position: fixed;
  left: 50%;
  top: 50%;
  margin-left: -9.4075rem;
  margin-top: -10.75rem;
  text-align: left;
  padding:0 2rem;
}
.alert .contactCard .name{
  margin-top: 1.5rem;
  margin-bottom: 4.5rem;
  font-size: 1rem;
  color: #fff;
  position: relative;
  text-indent: 2rem;
}
.alert .contactCard .name img{
  display: inline-block;
  width: 1.345rem;
  height: 1.345rem;
  position: absolute;
  top: 0.05rem;
  left: 0.2rem;
}
.contactWays{
  font-size: 0.875rem;
  color: #333333;
  position: relative;
  text-indent: 2.5rem;
  line-height: 1.2rem;
  margin-bottom: 1.5rem;
}
.contactWays p:nth-of-type(2){
  color: #666;
}
.contactWays img{
  display: inline-block;
  width: 1.125rem;
  height: 1rem;
  position: absolute;
  top: 0.05rem;
  left: 0.2rem;
}
.contactWays img:nth-of-type(2){
  width: 1.125rem;
  height: 1.25rem;
}
.contactWays img:nth-of-type(3){
  width: 1.25rem;
  height: 1.03rem;
}
.foot-close{
  width: 2.19rem;
  height: 3.5rem;
  position: absolute;
  right: 1.5rem;
  bottom: 0;
}
.foot-close img{
  width: 100%;
  height: 100%;
}
.enter-home{
  position: fixed;
  top: 6.5rem;
  right: 0;
  width: 5.655rem;
  height: 2.065rem;
}
.enter-home img{
  width: 100%;
  height: 100%;
}
.delete{
  width: 0.94rem;
  height: 0.94rem;
}
</style>
