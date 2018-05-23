<template>
  <div class="aside-container">
    <el-row class="tac">
      <el-col>
        <h3>欢迎您使用Star导航</h3>
        <h5>正在努力制作中.</h5>
        <el-menu
          default-active="0"
          class="el-menu-vertical-demo"
          background-color="#757F9A"
          text-color="#fff"
          active-text-color="#FFFF00">
          <el-menu-item v-for="(navTitle, index) in navTitleList" :index="String(index)" :key="navTitle.type" @click="chooseNavType(navTitle.type)">
            <i class="el-icon-star-on"></i>
            <span slot="title">{{ navTitle.name }}</span>
          </el-menu-item>
        </el-menu>
      </el-col>
    </el-row>
    <div class="friend-site">
      <a href="http://www.timrchen.site" target="_blank">
        <img src="https://image.timrchen.site/logo/timrchen_logo.png">
      </a>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Aside',
  data () {
    return {
      navTitleList: [
        {
          'name': '热门网址',
          'type': 'hot'
        },
        {
          'name': '媒体综合',
          'type': 'media'
        },
        {
          'name': '实用工具',
          'type': 'tool'
        },
        {
          'name': '有趣论坛',
          'type': 'forum'
        }
      ],
    }
  },
  methods: {
    chooseNavType: function (tab) {
      this.$emit('update:currentTab', tab)
    }
  },
  mounted: function () {
    this.$emit('update:currentTab', 'hot')
  }
}
</script>

<style scoped>
  .aside-container {
    position: relative;
    height: 100%;
    color: #fff;
  }
  .el-menu {
    border-right: none!important;
  }

  .friend-site {
    width: 100%;
    position: absolute;
    bottom: 0;
  }

  .friend-site a {
    text-decoration: none;
    color: #fff;
  }

  .friend-site a img {
    width: 100%;
    height: 100%;
  }

</style>
