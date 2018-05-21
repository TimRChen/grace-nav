<template>
  <div class="main-container">
    <ul class="url-list">
      <li class="url-card-container" v-for="urlData in currentCollection" :key="urlData.name">
        <a :href="urlData.url" target="_blank">
          <div class="card">
            <div class="url-title">{{ urlData.name }}</div>
            <div class="url-subtitle">{{ urlData.description }}</div>
          </div>
        </a> 
      </li>
    </ul>
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
  .url-list {
    width: 100%;
    list-style: none;
    display: flex;
    flex-flow: wrap;
    align-items: center;
  }
  .url-card-container {
    width: 20%;
    height: 60px;
    margin: 10px;
  }
  .url-card-container > a {
    text-decoration: none;
  }

  .card {
    display: flex;
    flex-flow: column;
    justify-content: center;
    background-color: #fff;
    border-radius: 4px;
    height: 100%;
  }

  .card .url-title {
    color: #000;
    font-size: 16px;
    margin-bottom: 4px;
  }
  .card .url-subtitle {
    color: #888888;
    font-size: 14px;
  }
</style>
