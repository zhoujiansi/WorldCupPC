<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<script>
require('vue-swipe/dist/vue-swipe.css');
import {setStorage , getStorage} from './server/localStorage'
import { mapState,mutations } from 'vuex'
import {getAdverList} from './utils/service'

export default {
  name: 'app',
  computed: mapState(['utm_source','publicfrom']),
  created (){
    // var url = location.search;
    // console.log("++++++",url);
    // var _request = {
    //     QueryString: function(val) {
    //         var uri = window.location.href;
    //         var re = new RegExp("" + val + "\=([^\&\?\#]*)", "ig");
    //         return ((uri.match(re)) ? (uri.match(re)[0].substr(val.length + 1)) : null);
    //     }
    // }
    // var utm_source= _request.QueryString('utm_source')||"";
    // var publicfrom= _request.QueryString('publicfrom')||"";
    // this.$store.commit('SET_UTM_SOURCE',utm_source)
    // this.$store.commit('SET_PUBLICFROM',publicfrom)
    // console.log("app.vue ",utm_source,publicfrom)
    this.getAdverList();
  },
  methods: {
    // mapMutations(['updateUserBind', 'updateUserPaid', 'updateUserPhone'])
    async getAdverList () { //获取广告数据
      const parm = {
        type: "电脑端",
      }
      let result = await getAdverList(parm)
      let data = result.data;
      console.log("***APP.vue-adver***",data.data)
      this.$store.commit('SET_ADVER',data.data)
    },
  }
}
</script>

<style>
html, body ,#app{
  width:1000px;
  /*height:100%;*/
  margin: 0 auto;
  overflow-x: hidden;
  overflow-y: auto;
}

/*设置滚动条宽度*/
html::-webkit-scrollbar{
  width:0px;
  -webkit-overflow-scrolling: touch;
}
/*设置滚动条宽度*/
body::-webkit-scrollbar{
  width:0px;
  -webkit-overflow-scrolling: touch;
}
body {
    background-color: #f0eff4;
}

h1, h2, h3, h4, h5, h6, p, ul, ol, dl, dd, fieldset, textarea, pre, img {
    margin: 0;
    padding: 0;
    list-style: none;
}
input,textarea{
   margin: 0;
   padding: 0;
   background:none;  
   outline:none;  
   border:0px;  
} 
</style>
