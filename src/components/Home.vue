<template>
  <el-container class="home_container">
    <el-header>
      <div class="home_title">BING BLOG</div>
      <div class="home_userinfoContainer">
        <el-dropdown @command="handleCommand">
  <span class="el-dropdown-link home_userinfo">
    {{currentUserName}}<i class="el-icon-arrow-down el-icon--right home_userinfo"></i>
  </span>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item command="sysMsg">系统消息</el-dropdown-item>
            <el-dropdown-item command="MyArticle">我的文章</el-dropdown-item>
            <el-dropdown-item command="MyHome">个人主页</el-dropdown-item>
            <el-dropdown-item command="logout" divided>退出登录</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </div>
    </el-header>
    <el-container>
      <el-aside width="200px">
        <el-menu
          default-active="0"
          class="el-menu-vertical-demo" style="background-color: #ECECEC" router>
          <template v-for="(item,index) in this.$router.options.routes" v-if="!item.hidden">
            <el-submenu :index="index+''" v-if="item.children.length>1" :key="index">
              <template slot="title">
                <i :class="item.iconCls"></i>
                <span>{{item.name}}</span>
              </template>
              <el-menu-item v-for="child in item.children" v-if="!child.hidden" :index="child.path" :key="child.path">
                {{child.name}}
              </el-menu-item>
            </el-submenu>
            <template v-else>
              <el-menu-item :index="item.children[0].path">
                <i :class="item.children[0].iconCls"></i>
                <span slot="title">{{item.children[0].name}}</span>
              </el-menu-item>
            </template>
          </template>
        </el-menu>
      </el-aside>
      <el-container>
        <el-main>
          <el-breadcrumb separator-class="el-icon-arrow-right">
            <el-breadcrumb-item class="link" :to="{ path: '/home' }">首页</el-breadcrumb-item>
            <el-breadcrumb-item v-text="this.$router.currentRoute.name"></el-breadcrumb-item>
          </el-breadcrumb>
          <keep-alive>
            <router-view v-if="this.$route.meta.keepAlive"></router-view>
          </keep-alive>
          <router-view v-if="!this.$route.meta.keepAlive"></router-view>
        </el-main>
      </el-container>
    </el-container>
    <el-footer>
      <el-row>
        <el-col :span="6"><div class="grid-content bg-purple">Yuquan You</div></el-col>
        <el-col :span="6"><div class="grid-content bg-purple-light">Haohui Zhang</div></el-col>
        <el-col :span="6"><div class="grid-content bg-purple">Zunye Zou</div></el-col>
        <el-col :span="6"><div class="grid-content bg-purple-light">Shu Zhang</div></el-col>
      </el-row>
      <el-row>
        <el-col :span="6"><div class="grid-content bg-purple">3118005345</div></el-col>
        <el-col :span="6"><div class="grid-content bg-purple-light">3118005347</div></el-col>
        <el-col :span="6"><div class="grid-content bg-purple">3118005350</div></el-col>
        <el-col :span="6"><div class="grid-content bg-purple-light">3118005348</div></el-col>
      </el-row>
    </el-footer>
  </el-container>
</template>




<script>
  import {getRequest} from '../utils/api'
  export default{
    methods: {
      handleCommand(command){
        var _this = this;
        if (command == 'logout') {
          this.$confirm('注销登录吗?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
          }).then(function () {
            getRequest("/logout")
            _this.currentUserName = '游客';
            _this.$router.replace({path: '/'});
          }, function () {
            //取消
          })
        }
      }
    },
    mounted: function () {
      var _this = this;
      getRequest("/currentUserName").then(function (msg) {
        _this.currentUserName = msg.data;
      }, function (msg) {
        _this.currentUserName = '游客';
      });
    },
    data(){
      return {
        currentUserName: ''
      }
    }
  }
</script>
<style>
  .home_container {
    height: 100%;
    position: absolute;
    top: 0px;
    left: 0px;
    width: 100%;
  }

  .el-header {
    background-color: #ececec;
    color: #333;
    text-align: center;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .el-aside {
    background-color: #ECECEC;
  }

  .el-main {
    background-color: #fff;
    color: #000;
    text-align: center;
  }

  .home_title {
    color: #110b0b;
    font-size: 30px;
    display: inline;

  }

  .link:hover{
    color: #acacac !important;
  }

  .home_userinfo {
    color: #111111;
    cursor: pointer;
  }

  .home_userinfoContainer {
    display: inline;
    margin-right: 30px;
  }

 .el-breadcrumb__inner is-link:active{
   color: #acacac;
 }
  .el-breadcrumb__inner a,.el-breadcrumb__inner.is-link:hover{
    color: #acacac;
    cursor: pointer;
  }
  breadcrumb__inner.is-link:hover {
    color: #acacac;
    cursor: pointer;
  }

 .is-link:hover{
    color: #acacac;
  }


  .element.style {
    color: rgb(6, 80, 156);
    cursor: pointer;
  }
  .el-menu-item.is-active:hover {
    color: #acacac;
    cursor: pointer;
  }

  .el-menu-item.is-active {
    color: #666565;
  }
</style>
