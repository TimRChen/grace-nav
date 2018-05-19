<template>
  <div class="aside-container">
    <el-row class="tac">
      <el-col>
        <h5>Tim导航.</h5>
        <el-menu
          default-active="0"
          class="el-menu-vertical-demo"
          background-color="#757F9A"
          text-color="#fff"
          active-text-color="#78ffd6">
          <el-menu-item v-for="(navTitle, index) in navTitleList" v-bind:index="String(index)" v-bind:key="navTitle.type">
            <i class="el-icon-caret-right"></i>
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
          'name': '音乐',        
          'type': 'music'
        },
        {
          'name': '工具',        
          'type': 'tool'
        },
        {
          'name': '论坛',        
          'type': 'forum'
        }
      ]
    }
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
