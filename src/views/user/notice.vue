<template>
  <div class="notice-page">
    <div class="header">
      <div class="head">
        <a @click="$router.back()" class="back"></a>
        消息中心
      </div>
    </div>
    <div class="all-read">全部已读</div>
    <div class="nav-bar">
      <div @click="changeBar(0)">
        <img src="@/assets/xinguang/消息中心_slices/n_1.png" />
        <p>全部消息</p>
      </div>
      <div @click="changeBar(1)" v-if="footer === 'n2'">
        <img src="@/assets/xinguang/消息中心_slices/n_2.png" />
        <p>收益提醒</p>
      </div>
      <div @click="changeBar(2)" v-if="footer === 'n2'">
        <img src="@/assets/xinguang/消息中心_slices/n_3.png" />
        <p>财务通知</p>
      </div>
      <div @click="changeBar(3)">
        <img src="@/assets/xinguang/消息中心_slices/n_4.png" />
        <p>系统消息</p>
      </div>
    </div>
    <div class="notice-cont">
      <div
        class="notice-list"
        v-for="(v, k) in data.list"
        :key="k"
        @click="$router.push('/notice/' + v.id)"
      >
        <div class="icon-cont">
          <img
            :src="require('@/assets/xinguang/消息中心_slices/n_' + v.type + '.png')"
            class="icon"
          />
          <span class="point" v-if="!v.is_read"></span>
        </div>
        <div class="info">
          <div class="notice-right">
            <span class="title">{{ v.title }}</span>
            <span class="notice-time">{{ v.time | dateFormat }}</span>
          </div>
          <p class="notice-msg">{{ v.content }}</p>
        </div>
      </div>
    </div>
    <!-- <div class="fund_wrap">
            <table class="table">
                <tbody>
                <tr class="header">
                    <th class="first">状态</th>
                    <th>主题</th>
                    <th>时间</th>
                    <th class="last">操作</th>
                </tr>
                <tr v-for="(v,k) in data.list" :key="k">
                    <td v-if="!v.is_read" style="color: #00C800">未读</td>
                    <td v-if="v.is_read" style="color: #cccccc">已读</td>
                    <td>{{v.title}}</td>
                    <td>{{v.time|dateFormat}}</td>
                    <td>
                        <router-link class="button" :to="'/notice/' + v.id">查看</router-link>
                    </td>
                </tr>
                </tbody>
            </table>
        </div> -->
  </div>
</template>

<script>
import Fetch from '../../utils/fetch'
import moment from 'moment'

export default {
  name: 'login',
  data () {
    return {
      data: [],
      selectIndex: 0,
      footer: ''
    }
  },
  created () {
    this.$parent.footer(false)
  },
  filters: {
    dateFormat (time) {
      return moment(time).format('YYYY-MM-DD')
    }
  },
  mounted () {
    this.start()
    this.footer = localStorage.getItem('footer')
  },
  methods: {
    start () {
      Fetch('/user/notice').then(r => {
        this.data = r.data
      })

      console.log(this.data)
    },
    onClickLeft () {
      this.$router.back()
    },
    back () {
      this.$router.back()
    },
    changeBar (index) {
      this.selectIndex = index
    }
  }
}
</script>

<style lang="less" scoped>
.notice-page {
  background-color: #f5f5f5;
  font-family: PingFang SC;
  // font-weight: 400;
  min-height: 100%;

  .all-read {
    position: fixed;
    width: 86px;
    line-height: 29px;
    border: 1px solid #E9C58C;
    border-radius: 14px;
    text-align: center;
    font-size: 13px;
    font-weight: 400;
    color: #E9C58C;
    top: 10px;
    right: 15px;
    z-index: 99;
  }
  .nav-bar {
    width: 100%;
    height: 104px;
    background: #FFFFFF;
    display: flex;
    align-items: center;
    img {
      width: 44px;
      height: 44px;
    }
    p {
      font-size: 13px;
      font-weight: 500;
      color: #000000;
      margin-top: 10px;
    }
    div {
      flex: 1 0;
      height: 100%;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
    }
    // .select{
    //     font-size: 15px;
    //     font-weight: 500;
    //     color: #000000;
    //     position: relative;
    //     &::before{
    //         position: absolute;
    //         content: ' ';
    //         display: block;
    //         width: 50px;
    //         height: 2px;
    //         background-color: #0F79FF;
    //         border-radius: 1px;
    //         left: 50%;
    //         transform: translateX(-50%);
    //         top: 27px;
    //     }
    // }
  }
  .notice-cont {
    // width: 349px;
    // background-color: #FFFFFF;
    // border-radius: 7px;
    // margin-left: 13px;
    .notice-list {
      display: flex;
      margin-top: 13px;
      background-color: #fff;
      padding: 17px;
      .icon-cont {
        position: relative;
        margin-right: 16px;
        flex-shrink: 0;
        img {
          width: 43px;
          height: 43px;
        }
        .point {
          position: absolute;
          width: 6px;
          height: 6px;
          border-radius: 50%;
          background-color: red;
        }
      }
      .info {
        width: 0;
        flex-grow: 1;
        .notice-right {
          display: flex;
          justify-content: space-between;
          align-items: center;
          .title {
            font-size: 15px;
            font-weight: 400;
            color: #393939;
          }
          .notice-time {
            font-size: 12px;
            font-weight: 400;
            color: #aaaaaa;
          }
        }
        .notice-msg {
          font-size: 12px;
          font-weight: 400;
          color: #aaaaaa;
          margin-top: 10px;
          overflow: hidden;
          white-space: nowrap;
          text-overflow: ellipsis;
        }
      }
    }
  }
}
</style>
