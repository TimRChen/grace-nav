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
  </div>
</template>

<script>

export default {
  name: 'Aside',
  data () {
    return {
      navTitleList: [
        {
          'name': '热门',
          'type': 'hot'
        },
        {
          'name': '影音',
          'type': 'media'
        },
        {
          'name': '工具',
          'type': 'tool'
        },
        {
          'name': '论坛',
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
    height: 100%;
    color: #fff;
  }
  .el-menu {
    border-right: none!important;
  }

</style>
