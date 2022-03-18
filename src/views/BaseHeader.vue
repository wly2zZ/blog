<template>
  <el-header class="me-area">
    <el-row class="me-header">
      <el-col :span="4" class="me-header-left">
        <router-link to="/" class="me-title">
          <img src="../assets/img/logo.png" />
        </router-link>
      </el-col>

      <el-col v-if="!simple" :span="16">
        <el-menu
          :router="true"
          menu-trigger="click"
          active-text-color="#5FB878"
          :default-active="activeIndex"
          mode="horizontal"
        >
          <el-menu-item index="/"
            ><i class="el-icon-house"></i> 首页
          </el-menu-item>
          <el-menu-item index="/category/all"
            ><i class="el-icon-tickets"></i>文章分类</el-menu-item
          >
          <el-menu-item index="/tag/all"
            ><i class="el-icon-price-tag"> </i>标签</el-menu-item
          >
          <el-menu-item index="/archives"
            ><i class="el-icon-folder-add"></i>文章归档</el-menu-item
          >

          <el-col :span="4" :offset="3">
            <el-menu-item index="/write"
              ><i class="el-icon-edit"></i>写文章</el-menu-item
            >
          </el-col>
        </el-menu>
      </el-col>

      <template v-else>
        <slot></slot>
      </template>

      <el-col :span="4">
        <el-menu
          :router="true"
          menu-trigger="click"
          mode="horizontal"
          active-text-color="#5FB878"
        >
          <template v-if="!user.login">
            <el-menu-item index="/login">
              <el-button type="text">登录</el-button>
            </el-menu-item>
            <el-menu-item index="/register">
              <el-button type="text">注册</el-button>
            </el-menu-item>
          </template>

          <template v-else>
            <el-submenu index>
              <template slot="title">
                <img class="me-header-picture" :src="user.avatar" />
              </template>
              <el-menu-item index @click="logout"
                ><i class="el-icon-back"></i>退出</el-menu-item
              >
            </el-submenu>
          </template>
        </el-menu>
      </el-col>

      <!--  -->
      <el-autocomplete
        v-model="state"
        :fetch-suggestions="querySearchAsync"
        placeholder="请输入内容"
        @select="handleSelect"
      ></el-autocomplete>

    </el-row>
  </el-header>
</template>

<script>

import {showArticle} from '@/api/article'
export default {
  name: "BaseHeader",
  props: {
    activeIndex: String,
    simple: {
      type: Boolean,
      default: false,
    },
  },
  
  data() {
    return {
       articleVo: [],
        state: '',
        timeout:  null
    };
  },
  computed: {
    user() {
      let login = this.$store.state.account.length != 0;
      let avatar = this.$store.state.avatar;
      return {
        login,
        avatar,
      };
    },
  },
  methods: {
    logout() {
      let that = this;
      this.$store
        .dispatch("logout")
        .then(() => {
          this.$router.push({ path: "/" });
        })
        .catch((error) => {
          if (error !== "error") {
            that.$message({ message: error, type: "error", showClose: true });
          }
        });
    },  
    loadAll() {
        showArticle().then(data =>{
          this.articleVo = data.data;
          console.log(this.articleVo);
          return this.articleVo;
        })
        
      },
      querySearchAsync(queryString, cb) {
        var articleVo = this.articleVo;
        var results = queryString ? articleVo.filter(this.createStateFilter(queryString)) : articleVo;
        console.log("显示")
        console.log(articleVo)
        clearTimeout(this.timeout);
        this.timeout = setTimeout(() => {
          cb(results);
        }, 3000 * Math.random());
      },
      createStateFilter(queryString) {
        return (state) => {
          return (state.value.toLowerCase().indexOf(queryString.toLowerCase()) === 0);
        };
      },
      handleSelect(item) {
        console.log("选中")
        console.log(item.address);
      }
    },
    mounted(){
       this.articleVo = this.loadAll();
    }

};
</script>

<style>
.el-header {
  position: fixed;
  z-index: 1024;
  min-width: 100%;
  box-shadow: 0 2px 3px hsla(0, 0%, 7%, 0.1), 0 0 0 1px hsla(0, 0%, 7%, 0.1);
}

.me-title {
  margin-top: 10px;
  font-size: 24px;
}

.me-header-left {
  margin-top: 10px;
}

.me-title img {
  max-height: 2.4rem;
  max-width: 100%;
}

.me-header-picture {
  width: 36px;
  height: 36px;
  border: 1px solid #ddd;
  border-radius: 50%;
  vertical-align: middle;
  background-color: #5fb878;
}

/* 
 */
</style>
