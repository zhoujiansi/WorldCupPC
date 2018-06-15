<template>
  <div class="Rule-body">
    <login></login>
    <div class="mybottom">
      <div class="item" @click="select(1)">
        <img src="../assets/bottom1_1.png" class="bg1" v-show="selectIndex!=1"><img src="../assets/bottom2_1.png" class="bg1" v-show="selectIndex==1">
      </div>
      <div class="item" @click="select(2)">
        <img src="../assets/bottom1_2.png"  class="bg2" v-show="selectIndex!=2"><img src="../assets/bottom2_2.png"  class="bg2" v-show="selectIndex==2">
      </div>
      <div class="item" @click="select(3)">
        <img src="../assets/bottom1_3.png"  class="bg3" v-show="selectIndex!=3"><img src="../assets/bottom2_3.png"  class="bg3" v-show="selectIndex==3">
      </div>
      <div class="item" @click="select(4)">
        <img src="../assets/bottom1_4.png"  class="bg4" v-show="selectIndex!=4"><img src="../assets/bottom2_4.png"  class="bg4" v-show="selectIndex==4">
      </div>
      <div class="item" @click="select(5)">
        <img src="../assets/bottom1_5.png"  class="bg5" v-show="selectIndex!=5"><img src="../assets/bottom2_5.png"  class="bg5" v-show="selectIndex==5">
      </div>
      <div class="item" @click="select(6)">
        <img src="../assets/bottom1_6.png"  class="bg6">
      </div>
    </div>
    <swipe class="my-swipe" :speed="1000" :auto="4000" :show-indicators="false" >
      <swipe-item class="slide" v-for="adver in adverList" :key="adver.id">
        <img :src="adver.imageUrl" @click="skip(adver)">
      </swipe-item>
    </swipe>
    <div class="Rule-body-container">
      <div class="word" @click="show(1)">什么是竞猜积分？竞猜积分有什么用？</div>
      <div class="answer" v-show="showIndex==1">
        <img src="../assets/mao1.png" class="mao1">
        <img src="../assets/mao2.png" class="mao2">
        <p>1、用户通过电脑端或手机端，进入活动页面，注册登录后即可参与哈希世界杯有奖竞猜活动。</p>
        <p>2、每个用户注册成功后即可获赠初始积分1000分。</p>
        <p>3、用户可以使用积分进行世界杯赛事竞猜，最终在赛事结束后，可使用活动期间获得的竞猜积分进行礼品兑换</p>
        <p>4、活动总积分榜和乌鸦帝排行榜也将根据用户竞猜得分与竞猜负率进行名次排定，在活动结束后排行榜前20名用户均可获赠不同价值礼品。</p>
      </div>
      <div class="word" @click="show(2)">如何获取竞猜积分？</div>
      <div class="answer" v-show="showIndex==2">
        <img src="../assets/mao1.png" class="mao1">
        <img src="../assets/mao2.png" class="mao2">
        <p>1、每个用户注册成功后即可获赠初始积分1000分</p>
        <!-- <p>2、每个用户每日登录活动可获赠100积分。</p> -->
        <p>2、用户通过手机端进入活动页面分享活动至朋友圈可获赠200积分（每日最多可获赠200积分，活动期间累积不超过20次）</p>
        <!-- <p>4、成功邀请他人注册可获赠300积分。（最多获赠15000分）</p> -->
        <p>3、通过每日竞猜首页下方进入哈希天猫店铺可获赠50积分（每日最多获赠50积分）</p>
        <p>4、通过比赛竞猜获取积分（详见竞猜规则）</p>
      </div>
      <div class="word" @click="show(3)">如何下注竞猜？</div>
      <div class="answer" v-show="showIndex==3">
        <img src="../assets/mao1.png" class="mao1">
        <img src="../assets/mao2.png" class="mao2">
        <p>1、在每日竞猜板块进行世界杯所有赛事竞猜，可进行当日全部比赛竞猜</p>
        <p>2、用户参与竞猜时，最低下注1分，最高为你目前的积分数。每次竞猜同一场比赛只能投注一个选项，无法多选，每场开赛前10分钟停止下注。每次竞猜即默认有效，无法修改，因此提醒用户下注请谨慎。</p>
        <p>3、积分计算。竞猜开出后，如果你竞猜的选项和竞猜结果相符，获胜积分即为本次投注的积分。如果竞猜项和结果不符，则输掉全部投注积分。</p>
        <p>4、积分结算。系统将分两个时段进行积分结算。分别为比赛日的24点和9点。</p>
      </div>
      <div class="word" @click="show(4)">什么是总积分榜？上榜有什么奖励？</div>
      <div class="answer" v-show="showIndex==4">
        <img src="../assets/mao1.png" class="mao1">
        <img src="../assets/mao2.png" class="mao2">
        <p>1、系统将在每日09：00更新总积分榜和乌鸦帝排行榜，总积分榜、乌鸦帝榜将按照用户的积分、与竞猜场次负率为序排行。</p>
        <p>2、当用户总积分相同时，以参与场次从多到少排序。</p>
        <p>3、要参与竞猜就有输掉积分的风险，所以很多用户会发现自己竞猜了一些比赛后，积分没有那些只注册而不参与竞猜的人多。为规避这一问题，活动最后只有参与30场以上竞猜的用户，才能入围最后积分榜。因此，还需要用户多多参与竞猜，才可入围总积分排行榜。</p>
        <p>4、总积分榜与乌鸦帝榜同在7月15日总决赛后不再进行排名更新。</p>
        <p>排行榜奖励</p>
        <p>活动结束后，总积分榜排名前二十的用户可获赠如下奖品</p>
        <p>第一名：预言帝套装（高档电脑背包+哈希五合一水质检测试纸+小米移动电源+松下护眼台灯+水质分析实用手册）</p>
        <p>第二名： 预测帝套装（高档电脑背包+松下护眼台灯+哈希五合一水质检测试纸）</p>
        <p>第三名：竞猜帝套装（松下护眼台灯+乐扣保鲜盒套装+哈希五合一水质检测试纸）</p>
        <p>第四-第十名：松下护眼台灯</p>
        <p>第十一-二十名：小米移动电源</p>
      </div>
      <!-- <div class="word" @click="show(5)">排行榜奖励</div>
      <div class="answer" v-show="showIndex==5">
        <img src="../assets/mao1.png" class="mao1">
        <img src="../assets/mao2.png" class="mao2">
        <p>活动结束后，总积分榜排名前二十的用户可获赠如下奖品</p>
        <p>第一名：预言帝套装（高档电脑背包+哈希五合一水质检测试纸+小米移动电源+松下护眼台灯+水质分析实用手册）</p>
        <p>第二名： 预测帝套装（高档电脑背包+松下护眼台灯+哈希五合一水质检测试纸）</p>
        <p>第三名：竞猜帝套装（松下护眼台灯+乐扣保鲜盒套装+哈希五合一水质检测试纸）</p>
        <p>第四-第十名：松下护眼台灯</p>
        <p>第十一-二十名：小米移动电源</p>
      </div> -->
      <div class="word" @click="show(6)">什么是乌鸦帝排行榜？上榜有什么奖励</div>
      <div class="answer" v-show="showIndex==6">
        <img src="../assets/mao1.png" class="mao1">
        <img src="../assets/mao2.png" class="mao2">
        <p>1、系统将在每日09：00更新总积分榜和乌鸦帝排行榜，总积分榜、乌鸦帝榜将按照用户的积分、与竞猜场次负率为序排行。</p>
        <p>2、乌鸦帝榜最后将按照用户猜错场数进行排名，猜错的场次越多排名越靠前。</p>
        <p>3、当用户乌鸦帝排行榜排名相同时，以积分从多到少排序。</p>
        <p>4、总积分榜与乌鸦帝榜同在7月15日总决赛后不再进行排名更新。</p>
        <p>排行榜奖励</p>
        <p>活动结束后，总积分榜排名前二十的用户可获赠如下奖品</p>
        <p>第一名：预言帝套装（高档电脑背包+哈希五合一水质检测试纸+小米移动电源+松下护眼台灯+水质分析实用手册）</p>
        <p>第二名： 预测帝套装（高档电脑背包+松下护眼台灯+哈希五合一水质检测试纸）</p>
        <p>第三名：竞猜帝套装（松下护眼台灯+乐扣保鲜盒套装+哈希五合一水质检测试纸）</p>
        <p>第四-第十名：松下护眼台灯</p>
        <p>第十一-二十名：小米移动电源</p>
      </div>
      <div class="word" @click="show(7)">如何用积分兑换礼品？</div>
      <div class="answer" v-show="showIndex==7">
        <img src="../assets/mao1.png" class="mao1">
        <img src="../assets/mao2.png" class="mao2">
        <p>1、用户使用积分进行礼品兑换需进入奖品兑换区，奖品兑换功能将在竞猜活动结束后开放（预计在7月16日开放）。</p>
        <p>2、同一用户最多可进行3次礼品兑换，同一礼品只可兑换1次</p>
      </div>
      <!-- <div class="word" @click="show(8)">积分兑换表</div>
      <div class="answer" v-show="showIndex==8">
        <img src="../assets/mao1.png" class="mao1">
        <img src="../assets/mao2.png" class="mao2">
        <p>1、用户使用积分进行礼品兑换需进入奖品兑换区，奖品兑换功能将在竞猜活动结束后开放（预计在7月16日开放）。</p>
        <p>2、同一用户最多可进行3次礼品兑换，同一礼品只可兑换1次</p>
      </div> -->
    </div>
    <div class="mask" v-show="shareshow">
      <img src="../assets/result_close.png" class="result_close" @click="closeshow()" />
      <div class="tips">
        <img src="../assets/share.png" class="share">
      </div>
    </div>
    <!-- <img class="gobuy" @click="vipClick" src="../assets/goBuy.jpg"/> -->
  </div>
</template>

<script>
import login from './Login.vue'
import {GetScheduleAllList} from '../utils/service'
import {formatDate} from '../utils/time'
import {setStorage , getStorage} from '../server/localStorage'
import { mapState } from 'vuex'
export default {
  name: 'Rule',
  data () {
      return {
        showIndex:0,
        isLoading:false,
        uid:null,
        selectIndex:2,
        shareshow:false
      }
  },
  computed: mapState(['userdata','adverList']),
  components:{
    login,
  },
  methods:{
    closeshow(){
      this.shareshow=false;
    },
    skip(item){
      // console.log(item);
      window.location.href=item.href;
    },
    show(index){
      this.showIndex = index;
    },
    select(index){
        this.selectIndex = index;
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
        else if(index==6){
          this.shareshow=true;
        }
      }
  },
  watch:{
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.Rule-body{
  width: 1000px;
  height: 1582px;
  /*background-color: #ffffff;*/
  background: url(../assets/rule_bg.jpg) no-repeat top center;
  -webkit-background-size: cover;
  background-size: cover;
  overflow-x: hidden;
  overflow-y: auto;
}
.Rule-body-container{
  width: 1000px;
  /*padding-top: 0.2rem;*/
  position: absolute;
  margin-top: 25px;
  /*bottom: 1rem;*/
  /*overflow: auto;*/
  text-align: center;
  padding: 0px 192.5px;
  box-sizing: border-box;
}
.Rule-body-container .word{
  width: 615px;
  height: 61px;
  line-height: 61px;
  color: #054574;
  font-size: 20px;
  background: url(../assets/rule_01.png) no-repeat top center;
  -webkit-background-size: cover;
  background-size: cover;
  text-align: center;
  z-index: 1;
  position: relative;
}
.answer{
  position: relative;
  width: 500px;
  box-sizing: border-box;
  margin-left: 57.5px;
  margin-top: -10px;
  padding: 20px;
  background-color: transparent;
  border:solid 1px rgba(255,255,255,0.6);
  box-shadow: inset 500px -5px 5px hsla(0,0%,37%,.6);               /* For Firefox3.6+ */
  box-shadow: inset 500px -5px 5px hsla(0,0%,37%,.6);           /* For Chrome5+, Safari5+ */
  box-shadow: inset 500px -5px 5px hsla(0,0%,37%,.6);  
  border-top-width: 0px;
  border-bottom-left-radius: 0.1rem;
  border-bottom-right-radius: 0.1rem;
  font-size: 16px;
  color: white;
  z-index: 3;
  text-align: left;
}
.answer .mao1{
  position: absolute;
  width: 77px;
  top: -65px;
  left: -20px;
  z-index: 9;
}
.answer .mao2{
  width: 58px;
  position: absolute;
  bottom: -50px;
  right: -20px;
  z-index: 9;
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
