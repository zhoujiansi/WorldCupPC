<template src="./WorldCup.html"></template>

<script>
  import login from '../Login.vue'
import {GetScheduleTimeList , GetScheduleListByID,SaveGuess, GetCrowRankList, GetRankList} from '../../utils/service'
import {formatDate} from '../../utils/time'
import {setStorage , getStorage} from '../../server/localStorage'
import { mapState } from 'vuex'

export default {
  name: 'WorldCup',
  data () {
    return {
      rankList:[],
      crowRankList:[],
      scheduleList:[],
      scheduleIndex:0,
      uid:null,
      guess:null,
      scheduleListById:[],
      sumcounts:null,
      captcha: '',
      isLoading:false,
      showTime:false,
      time:0,
      second:60,
      showPt:false,
      check1:true,
      check2:true,
      // page:"page0",
      selectIndex:1,
      xzjf:0,
      showCrow:false,
      showCounts:false,
      showCathectic:false,
      showSuccess:false,
      showMySchedule:false,
      shareshow:false
    }
  },
  computed: mapState(['userdata','adverList']),
  components:{
    login,
  },
  mounted: function() {  
    this.GetScheduleList();
    this.GetRankList();
    this.GetCrowRankList();
    let uid = getStorage("uid");
    if(uid){
      this.uid = uid;
      this.showMySchedule=true;
      this.GetScheduleListByID(this.uid);
    }
  },
  created (){
    // console.log("######adver######",this.adverList)
    // setTimeout(()=> {
    //   this.page="page1";
    // },500);
  },
  filters:{
    formatDate(time){
      let date = new Date(time.replace(/-/g, "/"));
      return  formatDate(date,"MM月dd日");
    },
    formatDate1(time){
      let date = new Date(time.replace(/-/g, "/"));
      return  formatDate(date,"hh:mm");
    }
  },
  methods:{
    closeshow(){
        this.shareshow=false;
      },
    showCrowRule(status){
        this.showCrow = status;
    },
    showCountsRule(status){
        this.showCounts = status;
    },
    skip(item){
      // console.log(item);
      window.location.href=item.href;
    },
    // 关闭下注弹窗
    showResultRule(status){
      this.showCathectic=status;
    },
    showSuccessStatus(status){
      this.showSuccess=status;
    },
    change(){
      console.log("***change***")
      if(this.scheduleIndex < this.scheduleList.length-1){
        this.scheduleIndex++;
      }
      else if(this.scheduleIndex==this.scheduleList.length-1){
        this.scheduleIndex=0;
      }
    },
    selectGuess(info){
      if(info){
        if(this.uid){
          this.showCathectic= true;
          this.guess=info;
        }
        else{
          alert("您还没有登录，请登录后再试");
          this.$router.push({name: 'Login'});
          return false;
        }
      }else{
        alert("请选择该比赛竞猜结果")
      }
    },
    // 开始竞猜
    startGuess(info){
      if(Number(this.xzjf)>0&&Number(this.xzjf)<Number(this.sumcounts)){
        this.showCathectic= false;
        const parm = {
          "ScheduleId":this.scheduleList[this.scheduleIndex].id, //赛程
          "pdataid":this.uid, //登录人id
          "guess":this.guess, //竞猜
          "counts":this.xzjf, //下注分数
        }
        console.log("***startGuess***",parm);
        this.SaveGuess(parm);
      }else{
        alert("请输入正确的积分");
      }
    },
    phb_btn () {
      this.$router.push({name: 'Result'});
    },
    // 下注
    async SaveGuess (parm) { //获取赛程
      this.isLoading=true
      let result = await SaveGuess(parm)
      let data = result.data;
      this.isLoading=false;
      if(data.success){
        this.showSuccess=true;
      }
      else{
        alert(data.message);
      }
      // this.scheduleList =data.data;
      console.log("***SaveGuess***",data);
    },
    async GetScheduleList () { //获取赛程
      this.isLoading=true
      let result = await GetScheduleTimeList()
      let data = result.data;
      this.isLoading=false;
      this.scheduleList =data.data;
      console.log(data);
    },
    async GetScheduleListByID (uid) { //获取赛程
      this.isLoading=true
      const parm = {
        pdataid: uid
      }
      let result = await GetScheduleListByID(parm)
      let data = result.data;
      this.isLoading=false;
      this.sumcounts=data.suncounts;
      var obj=[];
      console.log(data.data);
      data.data.forEach(function(item,index){
        let mydate = new Date(item.starttime);
        var date = formatDate(mydate,"MM月dd日");
        var s = obj.filter(function(it){
          return it.date==date;
        })
        if(s.length>0){
            s[0].scheduleList.push(item);
          }
        else{
          var obt = {date:date,scheduleList:[item]};
          obj.push(obt);
        }
      });
      console.log("***scheduleListById***",obj)
      this.scheduleListById =obj;
    },
    async GetRankList () { //获取赛程
        this.isLoading=true
        let result = await GetRankList()
        let data = result.data;
        this.isLoading=false;
        this.rankList =data.data;
        // console.log(data);
    },
    async GetCrowRankList () { //获取赛程
        this.isLoading=true
        let result = await GetCrowRankList()
        let data = result.data;
        this.isLoading=false;
        this.crowRankList =data.data;
    },
    myottomselect(){
      this.shareshow=true;
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
    }
  },
  watch:{
  }
}
</script>
<style scoped>
  @import "./style.css"
</style>
<!-- Add "scoped" attribute to limit CSS to this component only -->