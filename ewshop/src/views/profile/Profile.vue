<template>
<div id="box">
    <NavBar>
    <template v-slot:default>个人中心</template>
    </NavBar>
    <div class="user-box">
      <div class="user-info">
        <div class="info">
          <img src="~assets/images/logo.png" alt="">
          <div class="user-desc">
            <span>昵称:{{ user.name }}</span>
            <span>登录名:{{user.email}}</span>
            <span class="name">个性签名：xxxxxxxxxxxxx</span>
          </div>
        </div>
      </div>
      <ul class="user-list">
        <li class="van-hairline--bottom" @click="goTo('/collect')">
          <span>我的收藏</span>
          <van-icon name="arrow"/>
        </li>
        <li class="van-hairline--bottom" @click="goTo('/order')">
          <span>我的订单</span>
          <van-icon name="arrow"/>
        </li>
        <li class="van-hairline--bottom" @click="goTo('/setting')">
          <span>账号管理</span>
          <van-icon name="arrow"/>
        </li>
        <li class="van-hairline--bottom" @click="goTo('/address')">
          <span>地址管理</span>
          <van-icon name="arrow"/>
        </li>
        <li class="van-hairline--bottom" @click="goTo('/about')">
          <span>关于我们</span>
          <van-icon name="arrow"/>
        </li>
      </ul>

    </div>
    <div style="margin: 100px 15px">
      <van-button @click="tologout" round block color="#44BA80">退出登录</van-button>
    </div>
</div>
</template>
<script>
import NavBar from 'components/common/navbar/NavBar.vue';
import {ref,reactive,toRefs,onMounted} from "vue";
import { mylogout,getUser} from "network/user";
import {useRouter} from 'vue-router';
import { Toast } from "vant";
import {useStore } from 'vuex';
export default {
    name:'Profile',
    setup(){
      const router = useRouter();
      const store = useStore();
      const state = reactive({
        user:{}
      })
      onMounted(()=>{
        getUser().then(res=>{
          state.user = res;
        })
      })

      const tologout = () => {
        mylogout().then(res=>{
          if (res.status == '204'){
            Toast.success('退出成功...');
            window.localStorage.setItem('ewshop_token','');
            store.commit('setIsLogin',false);
            setTimeout(()=>{
              router.push({path:'/login'});
            },500)
          }
        })
      }
      //跳转方法
      const goTo = (path,query) => {
        router.push({path,query:query || {}});
      }
      return {
        tologout,
        ...toRefs(state),
        goTo
      }
    },
    components:{
        NavBar
    }
    
}
</script>
<style scoped lang="scss">
#box{
  background-color: #FCFCFC;
  height: 100vh;
}
.user-box{
  margin-top: 65px;
  .user-header{
    position: fixed;
    top: 0;
    left: 0;
    z-index: 10000;
    width: 100%;
    height: 44px;
    line-height: 44px;
    padding: 0 10px;
    color: #252525;
    background: #fff;
    border-bottom: 1px solid #dcdcdc;
    .user-name{
      font-size: 14px;
    }
  }
  .user-info{
    width:94%;
    margin:50px 10px 10px;
    height: 115px;
    background: linear-gradient(90deg,#31c7A7,#A1c7c7);
    box-shadow: 0 2px 5px #269090;
    border-radius: 6px;
    text-align: left;
    .info{
      position: relative;
      display: flex;
      width: 100%;
      height: 100%;
      padding: 25px 20px;
      img{
        width: 60px;
        height: 60px;
        border-radius: 50%;
        margin-top: 4px;
      }
      .user-desc{
        display: flex;
        flex-direction: column;
        margin-left: 10px;
        line-height: 20px;
        font-size: 14px;
        color: #fff;
        span{
          color: #fff;
          font-size: 14px;
          padding: 2px 0;
        }
      }
      .account-setting{
        position: absolute;
        top: 10px;
        right: 20px;
        font-size: 13px;
        color: #fff;
        .van-icon-setting-o{
          font-size: 16px;
          vertical-align: -3px;
          margin-left: 4px;
        }
      }
    }
  }
  .user-list{
    padding: 0 8px;
    margin-top: 40px;
    li{
      padding: 0 5px;
      height: 40px;
      line-height: 40px;
      display: flex;
      justify-content: space-between;
      font-size: 14px;
      .van-icon-arrow{
        margin-top: 13px;
      }
      margin: 20px 0 !important;
      background: #FFFFFF;
      border-radius: 3px;
    }
  }
}
</style>