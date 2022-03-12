<template>
  <el-card class="me-area" :body-style="{ padding: '16px' }">
    <div class="me-article-header">
      <b></b>
      <a @click="view(_id)" class="me-article-title">{{ title }}</a>
      <i v-if="weight > 0">
        <el-button class="me-article-icon" type="text"
          ><i class="el-icon-thumb">置顶</i></el-button
        >
      </i>
      <span
        class="me-pull-right me-article-count"
        v-if="this.article.authorId == this.$store.state.id"
      >
        <i
          class="el-icon-delete-solid"
          style="color: #a6a6a6"
          @click="removeArticle(_id)"
        >
        </i>
      </span>
      <span class="me-pull-right me-article-count">
        <i class="el-icon-chat-dot-square"></i>&nbsp;
        {{ commentCounts == NULL ? 0 : commentCounts }}
      </span>
      <span class="me-pull-right me-article-count">
        <i class="el-icon-view"></i>&nbsp;{{ viewCounts }}
      </span>
    </div>

    <div class="me-artile-description">
      {{ summary }}
    </div>
    <div class="me-article-footer">
      <span class="me-article-author">
        <i class="el-icon-user"></i>&nbsp;{{ author }}
      </span>

      <span class="me-article-tag">
        <i class="el-icon-price-tag"></i>&nbsp;
      </span>
      <el-tag v-for="t in tags" :key="t.tagName" size="mini">{{
        t.tagName
      }}</el-tag>

      <span class="me-pull-right me-article-count">
        <i class="el-icon-time"></i>&nbsp;{{ createDate | format }}
      </span>
    </div>
  </el-card>
</template>

<script>
import { formatTime } from "../../utils/time";
import { removeArticleById, viewArticle } from "@/api/article";
export default {
  name: "ArticleItem",
  props: {
    _id: String,
    weight: Number,
    title: String,
    commentCounts: Number,
    viewCounts: Number,
    summary: String,
    author: String,
    tags: Array,
    createDate: String,
  },
  created() {
    this.getArticle(this._id);
  },
  data() {
    return {
      article: {
        authorId: 0
      },
    };
  },
  methods: {
    view(_id) {
      this.$router.push({ path: `/view/${_id}` });
    },
    removeArticle(_id) {
      //this.$message('请去后台管理系统进行删除或游客无权限删除');
      this.$confirm('此操作将永久删除该文章, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
          
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });          
        });
    },
    getArticle(_id) {
      let that = this;
      viewArticle(_id).then((data) => {
        Object.assign(that.article, data.data);
      });
    },
  },
};
</script>

<style scoped>
.me-article-header {
  /*padding: 10px 18px;*/
  padding-bottom: 10px;
}

.me-article-title {
  font-weight: 900;
}

.me-article-icon {
  padding: 3px 8px;
}

.me-article-count {
  color: #a6a6a6;
  padding-left: 14px;
  font-size: 15px;
}

.me-pull-right {
  float: right;
}

.me-artile-description {
  font-size: 13px;
  line-height: 24px;
  margin-bottom: 10px;
}

.me-article-author {
  color: #a6a6a6;
  padding-right: 18px;
  font-size: 13px;
}

.me-article-tag {
  color: #a6a6a6;
  padding-right: 0px;
  font-size: 13px;
}

.el-tag {
  margin-left: 6px;
}
</style>
