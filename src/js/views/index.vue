<template>

  <wxc-tab-bar
    :tab-titles="tabTitles"
    :tab-styles="tabStyles"
    title-type="icon"
    @wxcTabBarCurrentTabSelected="wxcTabBarCurrentTabSelected">
    <!--The first page content-->
    <div class="item-container" :style="contentStyle">
      <text>111111{{info}}</text>
      <wxc-cell
        class="l-list"
        label="label"
        title="title"
        desc="这是简介"
        :has-arrow="false"
        @wxcCellClicked="wxcCellClicked"
        :has-margin="true"></wxc-cell>
    </div>

    <!--The second page content-->
    <div class="item-container" :style="contentStyle">

    </div>
  </wxc-tab-bar>
</template>

<script>
  import {
    WxcTabBar,
    WxcMinibar,
    WxcCell,
    Utils
  } from 'weex-ui';
  import Config from '../utils/config'

  let modal = weex.requireModule('modal')

  export default {
    name: "index",
    components:{
      WxcTabBar,
      WxcMinibar,
      WxcCell
    },
    data(){
      return {
        contentStyle:null,
        tabTitles: Config.tabTitles,
        tabStyles: Config.tabStyles,
        info:''
        // currentObject: WXEnvironment
      }
    },
    created () {

      let result = this.$storage.getSync('userInfo')
      if (result && result.id){ // 如果有用户信息，则渲染首页
        this.$notice.alert({
          title: '有有',
          message: '消息',
          okTitle: '确认',
          callback() {
            // 点击确认按钮的回调
          }
        })
        this.$navigator.setNavigationInfo({
          title: '首页',
          navShow: true,
          statusBarStyle: 'Default'
        })

        const tabPageHeight = Utils.env.getPageHeight();
        // If the page doesn't have a navigation bar
        // const tabPageHeight = env.deviceHeight / env.deviceWidth * 750;
        const { tabStyles } = this;
        this.contentStyle = { height: (tabPageHeight - tabStyles.height) + 'px' };

      } else { // 否则就跳转到登录页面
        /*
          PUSH右侧打开。
          PRESENT底部弹出。
         */
        this.$notice.alert({
          title: '木有',
          message: '消息',
          okTitle: '确认',
          callback() {
            // 点击确认按钮的回调
          }
        })
        this.$router.open({
          name: 'login',
          type: 'PUSH'
        })
      }
    },
    methods:{
      wxcTabBarCurrentTabSelected(e){
        this.$navigator.setNavigationInfo({
          title: this.tabTitles[e.page].title
        })

        this.$notice.toast({
          message: '消息'
        })
      },
      wxcCellClicked(){

      }
    }
  }
</script>

<style scoped>
  .item-container{
    padding-top: 100px;
    width: 750px;
    background-color: #f2f3f4;
    align-items: center;
    justify-content: center;
  }
  .l-list{
    width: 750px;
  }
</style>
