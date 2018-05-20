<template>
  <div id="app">
    <el-container>
      <el-aside width="200px">
        <tim-aside :current-tab.sync="chooseTab"></tim-aside>
      </el-aside>
      <el-main>
        <tim-main :choose-tab="chooseTab"></tim-main>
      </el-main>
    </el-container>
  </div>
</template>

<script>
import TimAside from './components/TimAside'
import TimMain from './components/TimMain'

export default {
  name: 'app',
  components: {
    TimAside,
    TimMain
  },
  data () {
    return {
      chooseTab: ''
    }
  },
  watch: {
    chooseTab: function (val) {
      console.log(val)
    }
  }
}
</script>

<style>
body {
  padding: 0;
  margin: 0;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.el-container {
  height: 100vh;
}

.el-aside {
  height: 100%;
  background-color: #757F9A;
  color: #fff;
  text-align: center;
}

.el-main {
  height: 100%;
  background: linear-gradient(to right, rgb(117, 127, 154), rgb(215, 221, 232));
  color: #fff;
  text-align: center;
}
</style>
