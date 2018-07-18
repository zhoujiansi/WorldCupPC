<template>
  <div class="Accept-body">
    <login :showLogin="showLogin" v-on:operate="operate"></login>
    <!-- <div class="top">
        <img src="../assets/top_bg.png" class="top_bg" />
    </div> -->
    <div class="mybottom">
      <div class="item" @click="bottomselect(1)">
        <img src="../assets/bottom1_1.png" class="bg1" v-show="selectIndex!=1"><img src="../assets/bottom2_1.png" class="bg1" v-show="selectIndex==1">
      </div>
      <div class="item" @click="bottomselect(2)">
        <img src="../assets/bottom1_2.png"  class="bg2" v-show="selectIndex!=2"><img src="../assets/bottom2_2.png"  class="bg2" v-show="selectIndex==2">
      </div>
      <div class="item" @click="bottomselect(3)">
        <img src="../assets/bottom1_3.png"  class="bg3" v-show="selectIndex!=3"><img src="../assets/bottom2_3.png"  class="bg3" v-show="selectIndex==3">
      </div>
      <div class="item" @click="bottomselect(4)">
        <img src="../assets/bottom1_4.png"  class="bg4" v-show="selectIndex!=4"><img src="../assets/bottom2_4.png"  class="bg4" v-show="selectIndex==4">
      </div>
      <div class="item" @click="bottomselect(5)">
        <img src="../assets/bottom1_5.png"  class="bg5" v-show="selectIndex!=5"><img src="../assets/bottom2_5.png"  class="bg5" v-show="selectIndex==5">
      </div>
      <div class="item" @click="myottomselect()">
        <img src="../assets/bottom1_6.png"  class="bg6">
      </div>
    </div>
    <swipe class="my-swipe" :speed="1000" :auto="4000" :show-indicators="false" >
      <swipe-item class="slide" v-for="adver in adverList" :key="adver.id">
        <img :src="adver.imageUrl" @click="skip(adver)">
      </swipe-item>
    </swipe>
    <img class="accept_02" src="../assets/accept_02.png"/>
    <img class="accept_01" src="../assets/accept_01.png" @click="show(true)"/>
    <!-- <img class="accept_03" src="../assets/accept_03.png"/> -->
    <div class="mask" v-show="showFlag">
      <img src="../assets/result_close.png" class="result_close" @click="show(false)" />
      <div class="tips">
        <img src="../assets/accept_04.png" class="accept_04"  />
        <div class="content">
          <div class="title">我的积分数:{{counts}}  已消耗积分:{{WorldCupUseCounts}} </div>
          <div class="item" v-for="(item, index) in prizeList" @click="select(item.id)">
            <img src="../assets/accept_07.png" v-show="!prizeIds.includes(item.id)"  class="sl left"/>
            <img src="../assets/accept_06.png" v-show="prizeIds.includes(item.id)" class="sl left"/>
            <span class="middle">{{item.prizeCounts}}积分</span>
            <span class="right">{{item.prizeName}}</span>
          </div>
          <img src="../assets/accept_05.png" class="accept_05"  @click="exchange()"/>
        </div>
      </div>
    </div>
    <div class="mask" v-show="shareshow">
      <img src="../assets/result_close.png" class="result_close" @click="closeshow()" />
      <div class="tips">
        <img src="../assets/share.png" class="share">
      </div>
    </div>
    <div class="mask" v-show="shareselect">
      <img src="../assets/result_close.png" class="result_close" @click="closeselect()" />
      <div class="tips">
        <img src="../assets/accept_08.png" class="accept_08"  />
          <div class="content">
            <div class="ex_01">
              <div class="box-input">
                  <div class="box-icon">
                    *姓名：
                  </div>
                  <div class="input-container">
                    <input type="text" v-model="name" placeholder="请输入姓名"/>
                  </div>
              </div>
              <div class="box-input">
                  <div class="box-icon">
                    *手机号码：
                  </div>
                  <div class="input-container">
                    <input type="tel" v-model="phone"  placeholder="请输入手机号码"/>
                  </div>
              </div>
              <div class="box-input">
                  <div class="box-icon">
                    *公司名称：
                  </div>
                  <div class="input-container">
                    <input type="text" v-model="company"  placeholder="请输入公司名称"/>
                  </div>
              </div>
              <div class="box-input">
                  <div class="box-icon">
                    *收件地址：
                  </div>
                  <div class="input-container">
                    <input type="text" v-model="zip" placeholder="请输入收件地址"/>
                  </div>
              </div>
              <div class="change">
                <div class="word">*您是否从事水质检测相关工作？</div>
                <div class="select">
                  <div class="t1" @click="show(true)">
                    <img src="../assets/no.png" class="icon" v-show="!IsShow" /><img src="../assets/yes.png" class="icon" v-show="IsShow" /><span>是</span></div>
                  <div class="t2" @click="show(false)">
                    <img src="../assets/no.png" class="icon" v-show="IsShow" /><img src="../assets/yes.png" class="icon" v-show="!IsShow" /><span>否</span></div>
                </div>
              </div>
              <div class="btndiv">
                <!-- <img src="../assets/ex_02.png" class="ex_02" @click="goback"/> -->
                <img src="../assets/ex_03.png" class="ex_03" @click="submit"/>
              </div>
            </div>
          </div>
      </div>
    </div>
    <div class="mask" v-show="showResult">
      <img src="../assets/result_close.png" class="result_close" @click="showResultRule(false)" />
      <div class="tips">
        <img src="../assets/ex_04.png" class="ex_04"  />
        <div class="content content1">
          恭喜您，您已成功兑换{{result}} 礼品将在1个月内寄出，因礼品寄送量较大如部分礼品缺货我们将以同等价值其他礼品进行补寄，还请理解。
          后期将有哈希客服与您电话核实相关礼品信息，请您注意接听来自西安的客服电话。
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import login from './Login.vue'
import {GetAllPrizeList,TMWorldCup,AcceptPrize} from '../utils/service'
import {formatDate} from '../utils/time'
import {setStorage , getStorage} from '../server/localStorage'
import { mapState } from 'vuex'
export default {
  name: 'Accept',
  data () {
      return {
        showLogin:false,
        prizeList:[],
        prizeIds:[],
        showFlag:false,
        isLoading:false,
        uid:null,
        dateTime:null,
        counts:null,
        WorldCupUseCounts:null,
        selectIndex:4,
        shareshow:false,
        shareselect:false,  //是否显示寄送信息
        showResult:false,  //是否显示兑奖结果信息
        IsShow:false, // 是从事该职业,默认不是
        name:null,
        company:null,
        zip:null,
        phone:null,
        data8:'否',
        result:"",
      }
  },
  computed: mapState(['userdata','adverList']),
  components:{
    login,
  },
  mounted: function() {  
      this.GetAllPrizeList();
      // let uid = getStorage("uid");
      let uid = sessionStorage.getItem('uid');
      this.counts = getStorage("counts");
      this.WorldCupUseCounts = getStorage("WorldCupUseCounts");
      if(uid){
        this.uid = uid;
      }
  },
  methods:{
    showResultRule(status){
      this.showResult=false;
    },
    operate(status){
      console.log("******operate******",status)
      this.showLogin=status;
    },
    closeshow(){
      this.shareshow=false;
    },
    closeselect(){
      this.shareselect=false;
    },
    skip(item){
      // console.log(item);
      this.TMWorldCup();
      window.open(item.href);
    },
    show(status){
      // alert("礼物兑换将在2018-07-16 08时开始");
      // return false;
      let uid = sessionStorage.getItem('uid');
      if(uid){
        this.uid = uid;
      }
      if(this.uid==null){
        alert("您还没有登录，请登录后再试");
        this.showLogin=true;
        return false;
      }
      else if(this.dateTime<"2018-07-16 08:00:00"){
        alert("礼物兑换将在2018-07-16 08时开始")
      }
      else{
        this.showFlag = status;
      }
    },
    select(id){
      if(this.prizeIds.includes(id)){
        this.prizeIds.splice(this.prizeIds.findIndex(v => v === id),1);
        // console.log(this.prizeIds.join(","))
      } else{
        this.prizeIds.push(id);
        // console.log(this.prizeIds.join(","))
      }
    },
    exchange(){
      if(this.prizeIds.length>0){
        if(this.dateTime<"2018-07-16 08:00:00"){
          alert("礼物兑换将在2018-07-16 08时开始")
        }
        else{
          this.shareselect=true;
          this.showFlag=false;
        }
        
      } else {
        alert("请选择礼品后兑换");
        return false;
      }
    },
    async submit () { //提交
      if(this.prizeIds==null || this.uid==null|| this.name==null
        || this.phone==null|| this.company==null|| this.zip==null|| this.data8==null){
        alert("请填好数据后提交");
        return false;
      }
      let checkUserPhone = /^1([358][0-9]|4[579]|66|7[0135678]|9[89])[0-9]{8}$/
      if (!checkUserPhone.test(this.phone)) {
        // console.log('手机号验证通过');
        alert("请输入正确的手机号");
        return false;
      }
        const params={
          "PrizeIds":this.prizeIds.join(","), //
          "pdataid":this.uid, //登录人id
          "name":this.name, //
          "phone":this.phone, //
          "company":this.company, //
          "zip":this.zip, //
          "data8":this.data8, //
        }
        console.log(params)
        this.isLoading=true
        let result = await AcceptPrize(params)
        let data = result.data;
        this.isLoading=false;
        // this.prizeList =data.data;
        if(data.success==false){
          alert(data.message);
        }
        else{
          this.result=data.result;
          this.showResult=true; // 显示提示
          this.shareselect=false;
          setStorage('counts',data.data.counts);// 用户数据
          setStorage('WorldCupUseCounts',data.data.WorldCupUseCounts);// 用户数据
        }
    },
    async TMWorldCup () { //获取赛程
        this.isLoading=true
        const parm = {
          pdataid: this.uid,
        }
        let result = await TMWorldCup(parm)
        let data = result.data;
        setStorage('counts',data.data.counts);// 用户数据
        this.isLoading=false;
    },
    async GetAllPrizeList () { //获取赛程
        this.isLoading=true
        let result = await GetAllPrizeList()
        let data = result.data;
        this.isLoading=false;
        this.prizeList =data.data;
        this.dateTime=data.dateTime;
    },
    myottomselect(){
      this.shareshow=true;
    },
    bottomselect(index){
        this.selectIndex = index;
        console.log(this.selectIndex,index)
        if(index==1){
          this.$router.push({name: 'WorldCup'});
        }
        else if(index==2){
          this.$router.push({name: 'Rule'});
        }
        else if(index==3){
          this.$router.push({name: 'Counts'});
        }
        else if(index==4){
          this.$router.push({name: 'Accept'});
        }
        else if(index==5){
          this.$router.push({name: 'Schedule'});
        }
      }
  },
  watch:{
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.Accept-body{
  width: 1000px;
  height: 1884px;
  /*background-color: #ffffff;*/
  background: url(../assets/Accept_bg.jpg) no-repeat top center;
  -webkit-background-size: cover;
  background-size: cover;
  overflow: hidden;
}
.Accept-body-container{
  width: 100%;
  margin-top: -0.2rem;
  padding-top: 0.2rem;
  position: absolute;
  top: 2rem;
  bottom: 1rem;
  overflow: auto;
}
.accept_02{
  width: 100%;
  margin-top: 20px;
}
.accept_01{
  position: absolute;
  width: 200px;
  margin-left: 400px;
  top: 1300px;
}
.accept_03{
  position: absolute;
  width: 5.16rem;
  margin-left:0.67rem; 
  top: 7.39rem;
}
.top{
    position: fixed;
    top:0px;
    left: 0px;
    width: 100%;
}
.top .top_bg{
  width: 100%;

}
.tips{
  position: relative;
  width: 1000px;
  top:5%;
  text-align: center;
  font-size: 16px;
}
.tips .accept_04{
  width: 420px;
  z-index: 5;
  position: relative;
}
.tips .ex_04{
  width: 472px;
  z-index: 5;
  position: relative;
}
.tips .accept_08{
  width: 416px;
  z-index: 5;
  position: relative;
}
.tips .content{
  width: 360px;
  height: 300px;
  margin-top: -0.3rem;
  margin-left:320px;
  background-color: white;
  border-radius: 0.1rem;
  padding-top: 0.3rem;
  position: relative;
  z-index: 1;
}
.tips .content1{
  padding-left: 20px;
  padding-right: 20px;
  font-size: 18px;
  height: 230px;
  box-sizing: border-box;
}
.tips .content .title{
  color: #0081cc;
  font-size: 20px;
  text-align: center;
  margin-bottom: 20px;
}
.tips .content .accept_05{
  width: 168px;
  position: absolute;
  bottom: 15px;
  left: 100px;
}
.tips .content .item{
  padding:0rem 10px; 
  display: flex;
  flex-direction: row;
  /*justify-content:space-between;*/
  align-items:flex-start;
  font-size: 16px;
}
.tips .content .item .left{
  /*width: 10%;*/
}
.tips .content .item .middle{
  width: 40%;
  text-align: left;
}
.tips .content .item .right{
  width: 50%;
  text-align: left;
}
.tips .ex_01{
  font-size: 16px;
}
.tips .box-input{
  display: flex;
  margin: 0 auto;
  margin-bottom:10px;
  width: 100%;
  height: 35px;
  line-height: 35px;
  /*margin-left: 10px;*/
  border-radius: 10px;
  
  background-color: rgba(255,255,255,0.6);
  box-sizing: border-box;
}
.tips .box-input .box-icon{
  width: 30%;
  padding-left: 0px;
  text-align: right;
}
.tips .box-input .input-container{
  width: 70%;
  padding-right: 20px;
  overflow: hidden;
  height: 35px;
  line-height: 35px;
  
  /*box-sizing: border-box;*/
  font-size: 0px;
}
.tips .box-input .input-container input{
  border:solid 3px rgb(85,188,246);
  height: 35px;
  line-height: 35px;
  width: 100%;
  box-sizing: border-box;
}
.tips .change{
  background-color: rgba(255,255,255,0.6);
  padding:5px 20px;
  width: 100%;
  /*height: 0.6rem;*/
  /*line-height: 0.6rem;*/
  /*margin-left: 20px;*/
  box-sizing: border-box;
  text-align: left;
}
.tips .change .select{
  margin-left: 10px;
  margin-top: 10px;
  margin-bottom: 10px;
  display: flex;
  flex-direction: row;
  /*justify-content:space-around;*/
  align-items:center;
}
.tips .change .select .t1{
  margin-right: 10px;
}
.tips .change .icon{
  width: 16px;
  margin-right: 10px;
}
.tips .btndiv{
  width: 100%;
  text-align: center;
  /*margin-top: 20px;*/
}
.tips .btndiv .ex_03{
  width: 100px;
}
.sl{
  width: 20px;
}
.result_close{
  width: 0.2rem;
  position: absolute;
  right: 1.05rem;
  top: 20%;
  z-index: 999;
}
.mask{
  position: fixed;
  width: 1000px;
  top: 0;
  bottom: 0;
  background-color: rgba(0,0,0,.5);
  z-index: 99;
}

.mybottom{
  /*position: fixed;*/
  display: flex;
  flex-direction: row;
  justify-content:space-between;
  align-items:flex-end;
  margin-top: 409px;
  width: 1000px;
  font-size: 0px;
  z-index: 9;
  padding: 0px 40px;
  box-sizing: border-box;
  border-bottom:solid white 2px;
}
.mybottom .item .bg1{
  width: 134px;
}
.mybottom .item .bg2{
  width: 135px;
}
.mybottom .item .bg3{
  width: 181px;
}
.mybottom .item .bg4{
  width: 152px;
}
.mybottom .item .bg5{
  width: 153px;
}
.mybottom .item .bg6{
  width: 135px;
}
.my-swipe {
  /*position: fixed;*/
  margin-top: 0px;
  height: 165px;
  width: 1000px;
  color: #fff;
  z-index: 9;
}
.slide img{
  width: 100%;
  height: 165px;
}
.share{
  width: 454px;
  z-index: 5;
  position: relative;
  box-sizing: border-box;
}
</style>
