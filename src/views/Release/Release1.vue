<template>
  <div class="assets">
    <myHead msg="发布矿机" backgroundColor="#fff"></myHead>
    <div class="asset-process">
      <div class="step">
        <img v-show="nameAndVersion == ''" src="./img/step1.png" alt="">
        <img v-show="nameAndVersion !== ''" src="./img/step1_1.png" alt="">
      </div>
     <div class="step-title">
       <div class="active">矿机信息</div>
       <!-- <div>公司信息</div> -->
       <div>联系方式</div>
     </div>
    </div>
    <div class="assets-des1">
      <div class="des-item">
        <div class="des-item-l">矿机名称+型号</div>
        <div class="des-item-r"><input v-model="nameAndVersion" type="text" placeholder="填写矿机名称和型号" maxlength="9"></div>
      </div>
      <div class="des-item">
        <div class="des-item-l">商品价格</div>
        <div class="des-item-r">
          <input v-model.number="price" onkeyup="clearNoNum(this)" type="number" placeholder="请输入数字">
          <span class="unit">元</span></div>
      </div>
      <div class="des-item" style="position:relative">
        <div class="des-item-l">针对币种</div>
        <div class="des-item-r pro-choose">
          <input type="text" :value="getLabel(coinType)" placeholder="请选择">
          <span :class="showCoinType?'upArrow':'downArrow'"></span>
        </div>
        <cell
        class="myCell"
        title="Animated"
        is-link
        :border-intent="false"
        :arrow-direction="showCoinType ? 'up' : 'down'"
        @click.native="showCoinType = !showCoinType"></cell>
      </div>


      <div class="pro-type slide" :class="showCoinType?'animate':''">
        <label v-for="item in coinTypeList">
          <input v-model="coinType" name="assetsType" type="radio" :value="item.key" />{{item.label}} 
        </label>
      </div>
      <div class="des-item">
        <div class="des-item-l">产品状态</div>
        <div class="des-item-r">
          <span class="unit">
            <select v-model="productStatus">
              <option disabled value="">请选择产品状态</option>
              <option v-for="i in productStatusList" :value="i.key">{{i.label}}</option>
            </select>
          </span>
        </div>
      </div>

      <div class="des-item">
        <div class="des-item-l">到货时间类型</div>
        <div class="des-item-r pro-type">
          <label v-for="item in arrivalTimeTypeList">
          <input v-model="arrivalTimeType" name="assetsType" type="radio" :value="item.key" />{{item.label}} 
        </label>
        </div>
      </div>
      <div class="des-item">
        <div class="des-item-l">具体到货时间</div>
        <div class="des-item-r">
        <calendar v-model="arrivalTime" title="" disable-past placeholder="点击选择日期"></calendar>
        </div>
      </div>
    </div>
    <div class="assets-des2">
      <div class="des-item">
        <div class="des-item-l">算力</div>
        <div class="des-item-r range">
          <input v-model.number="calculateStress" onkeyup="clearNoNum(this)" type="number" min="0" max="100"> <span class="unit">T&nbsp;</span>
        </div>
      </div>
      <div class="des-item">
        <div class="des-item-l">消耗</div>
        <div class="des-item-r range">
          <input v-model.number="consume" onkeyup="clearNoNum(this)" type="number" min="0" max="100"> <span class="unit">台</span>
        </div>
      </div>
      <div class="des-item">
        <div class="des-item-l">起售数量</div>
        <div class="des-item-r">
          <input v-model.number="saleNum" onkeyup="clearNoNum(this)" type="number" placeholder="请输入数字">
          <span class="unit">台</span></div>
      </div>
      <div class="des-item">
        <div class="des-item-l">有无托管</div>
        <div class="des-item-r">
          <!-- <input v-model.number="dailyPayAmount" onkeyup="clearNoNum(this)" type="number" maxlength="20" placeholder="请输入数字"> -->
          <span class="unit">
            <select v-model="trusteeship">
              <option disabled value="">请选择</option>
              <option value="1">是</option>
              <option value="0">否</option>
            </select>
          </span>
        </div>
      </div>
      
      <div class="des-item">
        <div class="des-item-l">是否配有官方电源</div>
        <div class="des-item-r">
          <!-- <input v-model.number="totalPayAmount" onkeyup="clearNoNum(this)" type="number" maxlength="20" placeholder="请输入数字"> -->
          <span class="unit">
            <select v-model="powerSupply">
              <option disabled value="">请选择</option>
              <option value="1">是</option>
              <option value="0">否</option>
            </select>
          </span>
        </div>
      </div>
      
    </div>
    <div class="assets-des3">
      <div class="des-item" style="height: 4.215rem;line-height: 4.215rem;">
        <div class="des-item-l" style="height: 2.6rem;">售后</div>
        <div class="des-item-r">
          <input :value="customerService==''?'':customerService.substr(0,10)+'...'" readonly="readonly" placeholder="下拉填写" />
          <span :class="showAfterSales?'upArrow':'downArrow'"></span>
          <cell
            class="myCell"
            title="Animated"
            is-link
            :border-intent="false"
            :arrow-direction="showAfterSales ? 'up' : 'down'"
            @click.native="showAfterSales = !showAfterSales"></cell>
        </div>
      </div>
      <div class="slide" :class="showAfterSales?'animate':''">
        <textarea v-model="customerService" type="text" placeholder="请填写200个字以内描述,至少50字" maxlength="200" minlength="50"></textarea>
      </div>

      <div class="des-item" style="height: 4.215rem;line-height: 4.215rem;">
        <div class="des-item-l" style="height: 2.6rem;">产品介绍</div>
        <div class="des-item-r">
          <input :value="productIntroduction==''?'':productIntroduction.substr(0,10)+'...'" readonly="readonly" placeholder="下拉填写" />
          <span :class="showPrdctIntro?'upArrow':'downArrow'"></span>
          <cell
            class="myCell"
            title="Animated"
            is-link
            :border-intent="false"
            :arrow-direction="showPrdctIntro ? 'up' : 'down'"
            @click.native="showPrdctIntro = !showPrdctIntro"></cell>
        </div>
      </div>
      <div class="slide" :class="showPrdctIntro?'animate':''">
        <textarea v-model="productIntroduction" type="text" placeholder="请填写200个字以内描述,至少50字" maxlength="200" minlength="50"></textarea>
      </div>

      <div class="des-item">
      </div>
    </div>
    <div class="next" @click="nextWay">下一步</div>

    
  </div>
</template>

<script>
import Lib from '@/assets/js/Lib'
import myHead from '@/components/myHead'
import { Cell,Calendar } from 'vux'
import $ from "jquery"

export default {
  name: 'ReleaseAssets',
  components: {
    myHead,
    Cell,
    Calendar
  },
  data () {
    return {
      productStatusList:[], //产品状态列表
      coinTypeList:[], //币种类型列表
      arrivalTimeTypeList:[], //到货时间类型列表
      nameAndVersion:'', //矿机名称+型号
      price:'',  //矿机价格
      coinType:'', //币种
      productStatus: '', //产品状态
      arrivalTimeType:null, //到货时间类型
      arrivalTime:'', //具体到货时间
      calculateStress:'',//算力
      consume:'', //消耗
      trusteeship: '', //有无托管 1是0否
      saleNum:'', //起售数量
      powerSupply: '', //是否配有官方电源 1是0否
      customerService:'', //售后
      productIntroduction:'', //产品介绍
      showAfterSales:false,
      showPrdctIntro:false,
      showCoinType: false,

      /*dailyPayAmount:'',
      totalPayAmount:'',*/
    }
  },
  watch:{
    arrivalTimeType(){
      console.log(this.arrivalTimeType);
    }
  },
  mounted(){
    this.productStatusList = JSON.parse(localStorage.productStatusList);
    this.coinTypeList = JSON.parse(localStorage.coinTypeList);
    this.arrivalTimeTypeList = JSON.parse(localStorage.arrivalTimeTypeList);
    if(localStorage.addPdParams != null){
      let a = JSON.parse(localStorage.addPdParams);
    }

    /* select颜色设置 */
    var unSelected = "#c2c2c2";
    var selected = "#333";
    $(function () {
        $("select").css("color", unSelected);
        $("option").css("color", selected);
        $("select").change(function () {
            var selItem = $(this).val();
            if (selItem == $(this).find('option:first').val()) {
                $(this).css("color", unSelected);
            } else {
                $(this).css("color", selected);
            }
        });
    })
  },
  methods:{
    nextWay(){
      if( 
         this.nameAndVersion == '' || this.nameAndVersion == null ||
         this.coinType == '' || this.coinType == null ||
         this.price == '' || this.price == null ||
         this.productStatus == '' || this.productStatus == null ||
         this.arrivalTimeType == '' || this.arrivalTimeType == null ||
         this.arrivalTime == '' || this.arrivalTime == null ||
         this.calculateStress == '' || this.calculateStress == null ||
         this.consume == '' || this.consume == null ||
         this.trusteeship == '' || this.trusteeship == null ||
         this.saleNum == '' || this.saleNum == null ||
         this.powerSupply == '' || this.powerSupply == null ||
         this.customerService == '' || this.customerService == null ||
         this.productIntroduction == '' || this.productIntroduction == null
        ){
        this.$vux.toast.text('内容请填写完整', 'middle');
      }else{
        /*if(this.customerService.length < 50 || this.productIntroduction < 50){
          this.$vux.toast.text('售后情况需超过50字', 'middle');
        }else{*/
          let addPdParams = {
            nameAndVersion : this.nameAndVersion,
            coinType : parseInt(this.coinType),
            price : parseInt(this.price),
            productStatus : this.productStatus,
            arrivalTimeType : this.arrivalTimeType,
            arrivalTime : this.arrivalTime,
            calculateStress : parseInt(this.calculateStress),
            consume : this.consume,
            trusteeship : this.trusteeship,
            saleNum : this.saleNum,
            powerSupply : this.powerSupply,
            customerService : this.customerService,
            productIntroduction : this.productIntroduction
          }
          localStorage.addPdParams = JSON.stringify(addPdParams);
          this.$router.push({ path: 'Release3' /*, query: { AorF: 'assets' }*/})
        /*}*/
      }
    },
    //资金资产类型数字转化为文字
    getLabel(key){
      var f = JSON.parse(localStorage.coinTypeList);
      for(let i in f){
        if(f[i].key == key) return f[i].label
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
textarea{
  width: 90%;
  outline: none;
  height: 3rem;
}
  .myCell{
    position: absolute;
    left:0;
    top:0;
    right:0;
    bottom: 0;
    opacity: 0;
  }
  .downArrow{
    display: inline-block;
    width: 0.75rem;
    height: 0.44rem;
    background: url("./img/choose_down.png") no-repeat center;
    background-size: 100% 100%;
  }
  .upArrow{
    display: inline-block;
    width: 0.75rem;
    height: 0.44rem;
    background: url("./img/choose_up.png") no-repeat center;
    background-size: 100% 100%;
  }
  ::-webkit-input-placeholder { /* WebKit browsers */
    font-size: 0.815rem;
    letter-spacing: 1px;
  }
  :-moz-placeholder { /* Mozilla Firefox 4 to 18 */
    font-size: 0.815rem;
    letter-spacing: 1px;
  }
  ::-moz-placeholder { /* Mozilla Firefox 19+ */
    font-size: 0.815rem;
    letter-spacing: 1px;
  }
  :-ms-input-placeholder { /* Internet Explorer 10+ */
    font-size: 0.815rem;
    letter-spacing: 1px;
  }
.assets{
  width: 100%;
  height: 41rem;
  background: #EFEFF4;
  margin: 0 auto;
}
.asset-process{
  width: 100%;
  height: 6.03rem;
  background: #fff;
  margin-top: 0.72rem;
  overflow: hidden;
}
.asset-process .step{
  width: 100%;
  height: 0.875rem;
  margin: 1rem auto;
  text-align: center;
}
.asset-process .step img{
  width: 75%;
}
.step-title{
  display: flex;
  flex-direction: row;
}
.step-title div{
  flex-grow: 1;
  margin-top: 1rem;
  font-size: 0.75rem;
  color: #333333;
  text-align: center;
}
.step-title div.active{
  color: #4083FF;
}
.assets-des1{
  width: 100%;
 /* height: 15.625rem;*/
  background: #fff;
  margin-top: 0.625rem;
}
.des-item{
  position: relative;
  display: flex;
  flex-direction: row;
  margin-left: 0.5rem;
  padding:0 2rem 0 1.2rem;
  height: 3.5rem;
  line-height: 3.125rem;
  border-bottom: 0.06rem solid #E6E6E6;
}
.assets-des1 .des-item:nth-of-type(1){
  border-bottom: 0.06rem solid #E6E6E6;
}
.des-item .des-item-l{
  text-align: left;
  flex-grow: 1;
  font-size: 0.94rem;
  color: #1A1A1A;
}
.des-item .des-item-r{
  text-align: right;
  flex-grow: 3;
  font-size: 0.815rem;
  color: #C2C2C2;
}
.des-item .des-item-r input{
  border:none;
  outline: none;
  text-align: right;
  /* width: 90%; */
  height: 100%;
}
.assets-des2{
  width: 100%;
  /* height: 18rem; */
  background: #fff;
  margin-top: 0.625rem;
}
.assets-des2 .range input{
  width:2.595rem;
  height:1.44rem;
  padding-right: 0.3rem;
  border: 0.06rem solid #333;
  outline: none;
}
.assets-des3{
  width: 100%;
  height: 7rem;
  background: #fff;
  border-top: solid 0.625rem #EFEFF4;
}
.next{
  width: 100%;
  max-width: 640px;
  height: 3.065rem;
  background: #4083FF;
  font-size: 1.065rem;
  color: #FFF;
  text-align: center;
  line-height: 3.065rem;
  position: fixed;
  bottom: 0;
}

.pro-type{
  display: block;
}
.pro-type input[type="radio"] {
  display: inline-block;
  width: 1.19rem;
  height: 1.19rem;
  border: 0.06rem solid #E6E6E6;
  border-radius: 50%;
  cursor: pointer;
  margin-right: 0.2rem;
  outline: none;
}
.pro-type input:checked{
  background: url("./img/selected_one.png") no-repeat center;
  background-size: 100% 100%;
}
.pro-type label {
  display: inline-block;
  height: 3rem;
  line-height: 3rem;
  margin-right: 0.5rem;
}
.pro-choose span{
  margin-left: 0.5rem;
}
.downArrow{
  display: inline-block;
  width: 0.75rem;
  height: 0.44rem;
  background: url("./img/choose_down.png") no-repeat center;
  background-size: 100% 100%;
}
.upArrow{
  display: inline-block;
  width: 0.75rem;
  height: 0.44rem;
  background: url("./img/choose_up.png") no-repeat center;
  background-size: 100% 100%;
}
.myCell{
  position: absolute;
  left:0;
  top:0;
  right:0;
  bottom: 0;
  opacity: 0;
}
.sub-item {
  color: #888;
}
.slide {
  padding: 0 20px;
  overflow: hidden;
  max-height: 0;
  transition: max-height .5s cubic-bezier(0, 1, 0, 1) -.1s;
}
.animate {
  max-height: 9999px;
  transition-timing-function: cubic-bezier(0.5, 0, 1, 0);
  transition-delay: 0s;
}
.unit{
  margin-left: 0.5rem;
  color: #1A1A1A;
}
</style>
