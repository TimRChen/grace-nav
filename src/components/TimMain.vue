<template>
  <div class="main-container">
    <ul class="url-list">
      <li class="url-card-container" v-for="urlData in currentCollection" :key="urlData.name">
        <a :href="(editState !== true) ? urlData.url : '#'" :target="(editState !== true) ? '_blank' : ''">
          <div class="card">
            <div class="delete-icon" v-show="editState === true && !urlData.description" @click="deleteCard(urlData)">
              <i class="el-icon-close"></i>
            </div>
            <div class="url-title">{{ urlData.name }}</div>
            <div class="url-subtitle">{{ urlData.description }}</div>
          </div>
        </a> 
      </li>
      <li class="extra-card-container">
        <div class="extra-card" @click="createLocalFavoriteSite">
          <i class="el-icon-plus"></i>
        </div>
      </li>
      <li class="extra-card-container">
        <div class="extra-card" @click="editCard">
          <i class="el-icon-delete"></i>
        </div>
      </li>
    </ul>
    <footer class="site-copyright">
      Copyright © 2018 享受创造的快乐.
    </footer>
  </div>
</template>

<script>
import URL_Collection from '../assets/url_collection.json' // 导入网址Json文件

export default {
  name: 'Main',
  props: ['chooseTab'],
  data () {
    return {
      currentCollection: [],
      editState: false, // delete | normal
      hasDelete: false // 是否已点击删除
    }
  },
  methods: {
    createLocalFavoriteSite: function () {
      const vm = this
      let currentCollection = vm.currentCollection
      this.$prompt('请输入网址链接', '增加一个自定义网址', {
        confirmButtonText: '下一步',
        cancelButtonText: '取消',
        // eslint-disable-next-line
        inputPattern: /(http|ftp|https):\/\/[\w\-_]+(\.[\w\-_]+)+([\w\-\.,@?^=%&:/~\+#]*[\w\-\@?^=%&/~\+#])?/,
        inputErrorMessage: '网址格式不正确'
      }).then(({ value }) => {
        let siteObj = {
          "name": '',
          "url": value,
          "description": ''
        }
        this.$prompt('请输入网址名称', '提示', {
          confirmButtonText: '确认',
          cancelButtonText: '取消'
        }).then(({ value }) => {
          siteObj.name = value
          let userDefineCollection = [...currentCollection, ...[siteObj]]
          let chooseTab = vm.chooseTab
          let storageData = {
            'hot': { 'hot': userDefineCollection },
            'media': { 'media': userDefineCollection },
            'tool': { 'tool': userDefineCollection },
            'forum': { 'forum': userDefineCollection }
          }
          window.localStorage.setItem(`userDefineCollection_${chooseTab}`, JSON.stringify(storageData[chooseTab]))
          vm.currentCollection = userDefineCollection
          vm.showSuccess('创建成功')
        }).catch(() => {
          console.log('cancel')
        });
      }).catch(() => {
        console.log('cancel')
      });
    },
    editCard: function () {
      let vm = this
      let editState = vm.editState
      vm.editState = !editState
      let hasDelete = vm.hasDelete
      // 超时不进行删除操作，则恢复卡片状态
      let cancelTimer = setTimeout(function () {
        if (hasDelete === false) vm.editState = false
        clearTimeout(cancelTimer)
      }, 5000)
    },
    // 删除提示
    deleteTip: function () {
      this.hasDelete = true
      return this.$confirm('此操作将永久删除该片卡, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      })
    },
    /**
     * 删除自定义卡片
     * @argument urlData 自定义卡片数据
     */
    deleteCard: function (urlData) {
      this.deleteTip().then(res => {
        console.log(res)
        let currentCollection = this.currentCollection
        let index = currentCollection.findIndex(value => value.name === urlData.name)
        currentCollection.splice(index, 1)

        let chooseTab = this.chooseTab
        window.localStorage.removeItem(`userDefineCollection_${chooseTab}`)
        let storageData = {
          'hot': { 'hot': currentCollection },
          'media': { 'media': currentCollection },
          'tool': { 'tool': currentCollection },
          'forum': { 'forum': currentCollection }
        }
        window.localStorage.setItem(`userDefineCollection_${chooseTab}`, JSON.stringify(storageData[chooseTab]))
        this.currentCollection = currentCollection
        this.$message({
          type: 'success',
          message: '删除成功!'
        });
      }).catch(() => {
        console.log('取消删除')       
      });
    },
    showTip: function () {
      const vm = this
      let addtipTimer = setTimeout(function () {
        vm.$notify.info({
          title: '如何增加一条新的网址?',
          message: '点击\'+\'加号，在输入框中输入网址名称与链接即可',
          position: 'top-right'
        })
        clearTimeout(addtipTimer)
      }, 5000)
      let deleteTipTimer = setTimeout(function () {
        vm.$notify.warning({
          title: '如何删除自定义网址卡片?',
          message: '点击删除符号，并确认删除即可',
          position: 'top-right'
        })
        clearTimeout(deleteTipTimer)
      }, 8000)
    },
    showSuccess: function (msg) {
      this.$message({
        message: msg,
        type: 'success'
      })
    },
  },
  mounted () {
    const vm = this
    vm.showSuccess('欢迎使用Star导航')
    vm.showTip()
  },
  watch: {
    // 初始化网址集 && 选择tab
    chooseTab: function (val) {
      let userDefineSiteData = JSON.parse(window.localStorage.getItem(`userDefineCollection_${val}`))
      if (userDefineSiteData) {
        this.currentCollection = userDefineSiteData[val]
      } else {
        this.currentCollection = URL_Collection[val]
      }
    }
  }
}
</script>

<style scoped>
  .main-container {
    font-family: "Helvetica Neue",Helvetica,"PingFang SC","Hiragino Sans GB","Microsoft YaHei","微软雅黑",Arial,sans-serif;
    height: 100%;
    color: #fff;
    position: relative;
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
    position: relative;
  }

  .card:hover {
    box-shadow: 0 0 8px 0 rgba(0,0,0,.3);
  }

  .card:hover .url-subtitle {
    color: #000;
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

  .delete-icon {
    width: 18px;
    height: 18px;
    position: absolute;
    top: -6px;
    right: -6px;
    background-color: #888;
    border-radius: 10px;
    color: #fff;
  }

  .delete-icon:hover {
    background-color: red;
  }

  .extra-card-container {
    width: 20%;
    height: 60px;
    margin: 10px;
  }

  .extra-card {
    display: flex;
    flex-flow: column;
    justify-content: center;
    border-radius: 4px;
    height: 100%;
    border: 1px dashed #fff;
    border-radius: 4px;
    opacity: 0.7;
  }

  .extra-card:hover {
    cursor: pointer;
    opacity: 1;
  }

  .extra-card i {
    font-size: 30px;
  }

  footer.site-copyright {
    position: absolute;
    bottom: 0;
    width: 100%;
    height: 50px;
  }
</style>
