<template>
  <div id="app">
    <keep-alive>
      <div class="header" v-if="currentPath && title">
        <div class="head">
          <van-icon
            name="arrow-left"
            size="23"
            color="#000"
            @click="handleBack"
          />
          <!--<router-link :to="path" class="back"></router-link>-->
          <div>{{ title }}</div>
          <div></div>
        </div>
      </div>
    </keep-alive>
    <div style="flex: 1 0;overflow: scroll;">
      <router-view></router-view>
    </div>
    <div class="footer_nav" v-if="show_footer && footer_type === 'n2'">
      <div class="indexnav">
        <div class="nav_top btn-group">

          <router-link class="footer_item" to="/tree">
            <img
              :src="
                show_type == 'tree'
                  ? require('@/assets/xinguang/画板 1_slices/首页 拷贝 2@2x.png')
                  : require('@/assets/xinguang/画板 1_slices/首页 拷贝 3@2x.png')
              "
            />
            <span>首页</span>
          </router-link>

          <router-link class="footer_item" to="/item">
            <img
              :src="
                show_type == 'touzi'
                  ? require('@/assets/xinguang/画板 1_slices/理财标识 拷贝@2x.png')
                  : require('@/assets/xinguang/画板 1_slices/理财标识 拷贝 2@2x.png')
              "
            />
            <span>投资</span>
          </router-link>

          <router-link class="footer_item" to="/index"
            ><img
              :src="
                show_type == 'mission'
                  ? require('@/assets/xinguang/画板 1_slices/任务1 拷贝@2x.png')
                  : require('@/assets/xinguang/画板 1_slices/任务1 拷贝 2@2x.png')
              "
            /><span>任务</span></router-link
          >

          <router-link class="footer_item" to="/user"
            ><img
              :src="
                show_type == 'info'
                  ? require('@/assets/xinguang/画板 1_slices/我 的 拷贝@2x.png')
                  : require('@/assets/xinguang/画板 1_slices/我 的 拷贝 2@2x.png')
              "
            /><span>我的</span></router-link
          >
        </div>
      </div>
    </div>
    <div id="footer" v-if="show_footer && footer_type === 'n1'">
      <div class="indexnav">
        <div class="nav_top btn-group" style="justify-content: center">
          <router-link class="footer_item" to="/tree"
            ><img
              :src="
                show_type == 'tree'
                  ? require('@/assets/xinguang/画板 1_slices/首页 拷贝 2@2x.png')
                  : require('@/assets/xinguang/画板 1_slices/首页 拷贝 3@2x.png')
              "
            /><span>首页</span></router-link
          >
          <router-link class="footer_item" to="/index"
            ><img
              :src="
                show_type == 'mission'
                  ? require('@/assets/xinguang/画板 1_slices/任务1 拷贝@2x.png')
                  : require('@/assets/xinguang/画板 1_slices/任务1 拷贝 2@2x.png')
              "
            /><span>任务</span></router-link
          >
          <router-link class="footer_item" to="/user"
            ><img
              :src="
                show_type == 'info'
                  ? require('@/assets/xinguang/画板 1_slices/我 的 拷贝@2x.png')
                  : require('@/assets/xinguang/画板 1_slices/我 的 拷贝 2@2x.png')
              "
            /><span>我的</span></router-link
          >
        </div>
      </div>
    </div>
    <kefu></kefu>
  </div>
</template>

<script>
import { Dialog } from 'vant'
import Fetch from './utils/fetch'
import kefu from './utils/kefu.vue'
import md5 from 'js-md5'
import router from './router'
var path, title, currentPath
router.beforeEach((to, from, next) => {
  /* 路由发生变化修改页面title */
  console.log(from.path, to)
  path = from.path
  currentPath = to.meta.hiddenTitle ? false : true
  title = to.meta.title
  setTimeout(() => {
    window.$('body').scrollTop(0)
  })
  next()
})
export default {
  name: 'App',
  data () {
    return {
      footer_type: 'n1',
      show_footer: false,
      show_type: 'index',
      path: '/',
      title: '',
      currentPath: '/',
      idfa: ''
    }
  },

  components: {
    kefu
  },
  beforeUpdate () {
    this.path = path
    this.title = title
    this.currentPath = currentPath
    console.log(currentPath)
  },

  created () {
    var type = localStorage.getItem('footer')
    if (type) {
      this.footer_type = type
    }
  },
  mounted () {
    this.syncDevNo()
    this.timer = setInterval(() => {
      this.syncDevNo()
    }, 5000)
  },
  methods: {
    //是否显示底部
    footer: function (bool, show = 'index') {
      this.show_footer = bool
      this.show_type = show
    },
    setFooterType: function (type = 'n1') {
      console.log(type)
      this.footer_type = type
      localStorage.setItem('footer', type)
    },
    getFooterType: function () {
      return this.footer_type
    },
    syncDevNo () {
      if (this.title === '消息中心') {
        clearInterval(this.timer)
        return
      }
      Fetch('/index/sync', {}, null, false).then(r => {
        this.footer_type = r.data.footer
        // this.footer_type = 'n2';
        localStorage.setItem('footer', this.footer_type)
        if (r.data.is_open_notice_dialog) {
          this.openNoticeDialog()
        }
      })
    },
    openNoticeDialog () {
      Dialog.confirm({
        title: '提示',
        message: '您有新消息，是否前往查看?'
      })
        .then(() => {
          this.$router.push('/notice').catch(err => {
            err
          })
        })
        .catch(() => {
          return
        })
    },
    handleBack () {
      this.$router.go(-1)
    }
  }
}
</script>
<style lang="less" scoped>
.paddingTop {
  padding-top: 50px;
}
.indexnav {
  width: 100%;
  background: #fff;
  overflow: hidden;
  line-height: 30px;
  font-size: 12px;
  // margin-top: 24px;
  padding-top: 15px;

  .nav_top,
  .nav_bottom {
    width: 100%;
    margin: 0 auto;
    display: flex;
    justify-content: space-around;
    flex-wrap: nowrap;
    margin-bottom: 14px;
  }
}

.indexnav a {
  text-align: center;
  width: 23.466%;
  font-size: 12px;
  display: flex;
  flex-direction: column;
}

.indexnav a span {
  width: 100%;
  display: block;
  line-height: 16px;
  margin-top: 4px;
  font-size: 13px;
}

.indexnav a img {
  width: 24px;
  height: 24px;
  margin: 0 auto;
  display: block;
  overflow: hidden;
}

#app {
  height: 100%;
  width: 100%;
  margin: 0 auto;
  display: flex;
  flex-direction: column;

  /* .kefu {
			position: fixed;
			right: 15px;
			top: 50%;
			transform: translateY(-50%);
			background: url('./assets/icons/online.png') no-repeat center;
			width: 35px;
			height: 138px;
			z-index: 2;
			background-size: contain;
		} */

  .footer_nav {
    width: 100vw;
    height: 70px;
    box-shadow: 0px -1px 5px rgba(89, 125, 172, 0.4);
    margin: 0 auto;
    z-index: 998;
    background-color: #fff;
    color: #7a7e83;
    border-left: 0 solid #fff;
    border-top: 0 solid #fff;
    border-right: 0 solid #fff;
    display: -webkit-box;
    display: -webkit-flex;
    display: flex;
    -webkit-justify-content: space-around;
    justify-content: space-around;
    border-top-right-radius: 0px;
    border-top-left-radius: 0px;
    box-sizing: border-box;
    overflow: hidden;
  }

  #footer {
    width: 100vw;
    height: 70px;
    box-shadow: 0px -1px 5px rgba(89, 125, 172, 0.4);
    margin: 0 auto;
    z-index: 998;
    background-color: #fff;
    color: #7a7e83;
    border-left: 0 solid #fff;
    border-top: 0 solid #fff;
    border-right: 0 solid #fff;
    display: -webkit-box;
    display: -webkit-flex;
    display: flex;
    -webkit-justify-content: center;
    justify-content: center;
    border-top-right-radius: 0px;
    border-top-left-radius: 0px;
    box-sizing: border-box;
    overflow: hidden;
  }

  #footer .footer_item {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
    -ms-flex-direction: column;
    flex-direction: column;
    -webkit-box-pack: center;
    -ms-flex-pack: center;
    justify-content: center;
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
    color: #4b4b4b;
    font-size: 12px;
  }

  #footer .footer_item img {
    width: 20px;
    height: 20px;
  }
}

.btn-group img {
  // background-color: #4CAF50; /* Green background */
  // border: 1px solid green; /* Green border */
  // color: white; /* White text */
  // padding: 10px 24px; /* Some padding */
  // cursor: pointer; /* Pointer/hand icon */
  // float: left; /* Float the buttons side by side */
}

/* Clear floats (clearfix hack) */
.btn-group:after {
  content: '';
  clear: both;
  display: table;
}

.btn-group img:not(:last-child) {
  border-right: none; /* Prevent double borders */
}

/* Add a background color on hover */
.btn-group img:hover {
  // opacity: 10px;
  // border-radius: 50px;
  //    background-color: #3e8e41;
}

#app ::-webkit-scrollbar {
  width: 0;
  height: 0;
}

.head {
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: #f5f5f5;
  padding: 10px 20px 0;
}
</style>

<style lang="less">
* {
  flex-shrink: 0;
  box-sizing: border-box;
}
</style>
