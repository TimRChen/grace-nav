<template>
  <div class="main-container">
    <el-row :gutter="20">
      <el-col :span="6" v-for="urlData in currentCollection" :key="urlData.name">
        <el-card shadow="hover">
          <a :href="urlData.url" target="_blank">
            <div class="url-title">{{ urlData.name }}</div>
            <div class="url-subtitle">{{ urlData.description }}</div>
          </a>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import URL_Collection from '../assets/url_collection.json' // 导入网址Json文件

export default {
  name: 'Main',
  props: ['chooseTab'],
  data () {
    return {
      currentCollection: []
    }
  },
  watch: {
    chooseTab: function (val) {
      this.currentCollection = URL_Collection[val]
    }
  }
}
</script>

<style scoped>
  .main-container {
    font-family: "Helvetica Neue",Helvetica,"PingFang SC","Hiragino Sans GB","Microsoft YaHei","微软雅黑",Arial,sans-serif;
    height: 100%;
    color: #fff;
  }
</style>
