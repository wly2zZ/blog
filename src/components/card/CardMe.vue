<template>
  <el-card>
    <h1 class="me-author-name">wly2zZ</h1>
    <div class="me-author-description">
      <span><i class="el-icon-location-outline"></i> &nbsp;广东&广州</span>
      <span><i class="el-icon-male"></i> &nbsp;Java练习生</span>
    </div>
    <div class="me-author-tool">
      <i @click="showTool(email)" :email="email.title" class="el-icon-s-promotion"></i>
      <i @click="showTool(qq)" :title="qq.title" class="iconfont icon-qq"></i>
      <i @click="showTool(bilibili)" :title="bilibili.title" class="iconfont icon-bilibili-fill"></i>
    </div>
  </el-card>

</template>

<script>
  export default {
    name: 'CardMe',
    data() {
      return {
        qq: {title: 'QQ', message: '1013381727'},
        bilibili: {
          title: '哔哩哔哩',
          message: 'https://space.bilibili.com/396837769?spm_id_from=333.788.0.0'
        },
        email:{title :'Eamil',message :'1013381727@qq.com'}
      }
    },
    methods: {
      showTool(tool) {
        this.$message({
          duration: 0,
          showClose: true,
          dangerouslyUseHTMLString: true,
          message: '<strong>' + tool.message + '</strong>'
        });
      }
    }
  }
</script>

<style scoped>
  .me-author-name {
    text-align: center;
    font-size: 30px;
    border-bottom: 1px solid #5FB878;
  }

  .me-author-description {
    padding: 8px 0;
  }

  .el-icon-male {
    padding-left: 16px;
  }

  .me-author-tool {
    text-align: center;
    padding-top: 10px;
  }

  .me-author-tool i {
    cursor: pointer;
    padding: 4px 10px;
    font-size: 30px;
  }
</style>
