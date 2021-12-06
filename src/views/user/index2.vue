<template>
  <div class="my-index-container">
    <div class="my-index-header">
      <div class="my-index-header-info-left">
        <div class="user-icon"></div>
        <div class="info-left-right">
          <div class="user-name">{{ data.mobile }}</div>
          <div class="user-vip">
            <template v-if="data.is_auth == 0">
              <span @click="handleUSerCertification"></span>
            </template>

            <template v-else>
              <i class="vip-icon"></i>
              <span class="vip-name">{{ data.vip_name }}</span>

              <div class="cheng">
                <i class=".cheng-icon"></i> 成长值：{{ www.value }}
              </div>
            </template>
          </div>
        </div>
      </div>
      <div class="my-index-header-info-right" @click="handleMyService('kefu')">
        <div class="line-service-icon"></div>
        <span>在线客服</span>
      </div>
    </div>

    <div class="my-user-info-list">
      <div class="my-user-info-list-item" @click="$router.push('/funds')">
        <div class="item-num">{{ data.money }}</div>
        <div class="item-name">账户余额</div>
      </div>
      <div class="my-user-info-list-item" @click="$router.push('/integral')">
        <div class="item-num">{{ data.integral }}</div>
        <div class="item-name">我的积分</div>
      </div>
      <div v-if="footerType === 'n2'" class="my-user-info-list-item">
        <div class="item-num">{{ data.ds_money }}</div>
        <div class="item-name">待收本金</div>
      </div>
      <div v-if="footerType === 'n2'" class="my-user-info-list-item">
        <div class="item-num">{{ data.ds_apr_money }}</div>
        <div class="item-name">待收利息</div>
      </div>
    </div>

    <div v-if="footerType === 'n2'" class="my-user-info-bottom">
      <div class="my-user-info-bottom-btn" @click="handleRecharge">
        <img
          src="~@/assets/xinguang/个人中心改 拷贝_slices/recharge@2x.png"
          alt=""
        />
        <span>账户充值</span>
        <van-icon class="avnicon" name="arrow" size="12" color="#656565" />
      </div>
      <div class="my-user-info-bottom-btn" @click="handleWithdraw">
        <img
          src="~@/assets/xinguang/个人中心改 拷贝_slices/提现明细@2x.png"
          alt=""
        />
        <span>账户提现</span>
        <van-icon class="avnicon" name="arrow" size="12" color="#656565" />
      </div>
    </div>

    <div class="user_info_box">
      <div
        class="info_item"
        v-if="footerType === 'n2'"
        @click="handleMyService('invest')"
      >
        <img
          src="~@/assets/xinguang/个人中心改 拷贝_slices/充值缴费 拷贝@2x.png"
          alt=""
        />
        <div>充值记录</div>
      </div>

      <div
        class="info_item"
        v-if="footerType === 'n2'"
        @click="handleMyService('cost')"
      >
        <img
          src="~@/assets/xinguang/个人中心改 拷贝_slices/提现 (1) 拷贝@2x.png"
          alt=""
        />
        <div>提现记录</div>
      </div>

      <div
        class="info_item"
        v-if="footerType === 'n2'"
        @click="handleMyService('touzi')"
      >
        <img
          src="~@/assets/xinguang/个人中心改 拷贝_slices/升级记录 拷贝@2x.png"
          alt=""
        />
        <div>投资记录</div>
      </div>

      <div
        class="info_item"
        v-if="footerType === 'n2'"
        @click="handleMyService('funds')"
      >
        <img
          src="~@/assets/xinguang/个人中心改 拷贝_slices/明细4-面型 拷贝@2x.png"
          alt=""
        />
        <div>资金明细</div>
      </div>

      <div
        class="info_item"
        v-if="footerType === 'n2'"
        @click="handleMyService('baoku')"
      >
        <img
          src="~@/assets/xinguang/个人中心改 拷贝_slices/理财 (1) 拷贝@2x.png"
          alt=""
        />
        <div>我的宝库</div>
      </div>

      <div class="info_item" @click="handleMyService('quanbao')">
        <img
          src="~@/assets/xinguang/个人中心改 拷贝_slices/优惠券 (1) 拷贝@2x.png"
          alt=""
        />
        <div>我的券包</div>
      </div>

      <div class="info_item" @click="handleMyService('notice')">
        <img
          src="~@/assets/xinguang/个人中心改 拷贝_slices/消息 拷贝@2x.png"
          alt=""
        />
        <div>站内消息</div>
      </div>

      <div class="info_item" @click="handleMyService('share')">
        <img
          src="~@/assets/xinguang/个人中心改 拷贝_slices/邀请 (2) 拷贝@2x.png"
          alt=""
        />
        <div>邀请好友</div>
      </div>
    </div>

    <section class="menu_box">
      <div
        class="item"
        v-if="footerType === 'n2'"
        @click="handleMyService('about')"
      >
        <img
          src="~@/assets/xinguang/个人中心改 拷贝_slices/关于我们@2x.png"
          alt=""
        />
        <span>关于我们</span>
      </div>
      <div class="item" @click="handleMyService('protocol')">
        <img
          src="~@/assets/xinguang/个人中心改 拷贝_slices/支持多种协议@2x.png"
          alt=""
        />
        <span>服务协议</span>
      </div>
      <div class="item" @click="handleMyService('privacy')">
        <img
          src="~@/assets/xinguang/个人中心改 拷贝_slices/协议@2x.png"
          alt=""
        />
        <span>隐私协议</span>
      </div>
      <div class="item" @click="handleMyService('account')">
        <img
          src="~@/assets/xinguang/个人中心改 拷贝_slices/wxb账户@2x.png"
          alt=""
        />
        <span>账户信息</span>
      </div>
    </section>

    <div class="log-out" @click="logout">退出登录</div>
  </div>
</template>

<script>
import Fetch from '../../utils/fetch'
import Vue from 'vue'
import { NoticeBar } from 'vant'

Vue.use(NoticeBar)

export default {
  name: 'user',
  data () {
    return {
      www: [],
      data: {},
      /* 待删除 */
      bg: '',
      footerType: 'n1',

      config: {},
      /* 待删除 */
      infoListOne: [
        {
          key: 'notice',
          name: '站内消息',
          style: { width: '39px', height: '39px' }
        },
        {
          key: 'share',
          name: '邀请好友',
          subTitle: '邀请好友获得奖励',
          style: { width: '39px', height: '39px' }
        },
        {
          key: 'account',
          name: '账户信息',
          subTitle: '修改登陆密码和支付密码',
          style: { width: '39px', height: '39px' }
        },
        {
          key: 'about',
          name: '关于我们',
          style: { width: '39px', height: '39px' }
        }
      ],
      infoListTwo: [
        {
          key: 'quanbao',
          name: '优惠券',
          subTitle: '7张',
          style: { width: '39px', height: '39px' }
        },
        {
          key: 'baoku',
          name: '我的宝库',
          style: { width: '39px', height: '39px' }
        },
        {
          key: 'protocol',
          name: '服务协议',
          style: { width: '39px', height: '39px' }
        },
        {
          key: 'privacy',
          name: '隐私协议',
          style: { width: '39px', height: '39px' }
        }
      ],
      infoListThree: [
        {
          key: 'logout',
          name: '退出登录',
          style: { width: '39px', height: '39px' }
        }
      ],
      infoListFour: [
        {
          key: 'invest',
          name: '充值记录',
          style: { width: '20px', height: '18px' }
        },
        {
          key: 'cost',
          name: '提现记录',
          style: { width: '16px', height: '16px' }
        },
        {
          key: 'touzi',
          name: '投资记录',
          style: { width: '18px', height: '15px' }
        },
        {
          key: 'funds',
          name: '资金明细',
          style: { width: '15px', height: '15px' }
        }
      ],
      infoListFive: [
        {
          key: 'logout',
          name: '退出登录',
          style: { width: '16px', height: '16px' }
        }
      ],
      infoListOther: [
        {
          key: 'order',
          name: '商城订单',
          style: { width: '14px', height: '16px' }
        },
        {
          key: 'plantTree',
          name: '种树记录',
          style: { width: '16px', height: '17px' }
        },
        {
          key: 'shuidi',
          name: '水滴记录',
          style: { width: '12px', height: '18px' }
        },
        {
          key: 'strategy',
          name: '种树攻略',
          style: { width: '14px', height: '17px' }
        },
        {
          key: 'auth',
          name: '实名认证',
          style: { width: '17px', height: '17 px' }
        },
        {
          key: 'alipay',
          name: '绑定支付宝',
          style: { width: '15px', height: '15px' }
        },
        {
          key: 'qiandao',
          name: '签到礼品',
          style: { width: '16px', height: '14px' }
        },
        {
          key: 'help',
          name: '帮助中心',
          style: { width: '18px', height: '18px' }
        }
      ]
    }
  },
  created () {
    console.log('this.$parent.getFooterType()', this.$parent.getFooterType())
    this.footerType = this.$parent.getFooterType()
    // if (this.$parent.getFooterType() === 'n2') {
    //   this.$router.replace('/user').catch(err => {
    //     err
    //   })
    // }
    this.$parent.footer(true, 'info')
  },
  mounted () {
    this.start()
  },
  methods: {
    start () {
      // 修复个别手机不能滚动到顶端
      setTimeout(() => {
        var element = document.getElementById('app')
        element.scrollIntoView()
      }, 0)
      document.body.scrollTop = 0
      document.documentElement.scrollTop = 0
      Fetch('/user/info').then(res => {
        this.data = res.data //res赋值
        this.www = res.data //res赋值

        console.log('个人信息', this.data) //接口内容打印
      })
      /* 待删除 */
      Fetch('/index/webconfig', {
        type: 'bg'
      }).then(res => {
        this.bg = 'url("' + res.data.login + '")'
      })
      Fetch('/index/webconfig', {
        type: 'web'
      }).then(res => {
        console.log('未知', res)
        this.config = res.data
      })
      /* 待删除 */
    },
    logout () {
      this.$parent.setFooterType('n1')
      localStorage.removeItem('token')
      this.$router.replace('/login')
    },
    // 实名认证
    handleUSerCertification () {
      this.$router.push({ name: 'auth' })
    },
    // 站内信
    handleUSernnotice () {
      this.$router.push({ name: 'notice' })
    },
    // 设置
    handleSetting () {
      // auth
      this.$router.push({ name: 'account' })
    },
    // 查看明细
    handleGoFunds () {
      this.$router.push({ name: 'funds' })
    },
    // 充值
    handleRecharge () {
      this.$router.push({ name: 'recharge' })
    },
    // 提现
    handleWithdraw () {
      this.$router.push({ name: 'cash' })
    },
    // 我的服务跳转
    handleMyService (name) {
      if (name === 'protocol') {
        this.handleGoProtocol(1)
        return
      }
      if (name === 'privacy') {
        this.handleGoProtocol(2)
        return
      }
      this.$router.push({ name: name })
    },
    // 协议
    handleGoProtocol (type) {
      if (type === 1) {
        this.$router.push({ path: this.config.user_contract_link })
      }
      if (type === 2) {
        this.$router.push({ path: this.config.user_contract_ys_link })
      }
    }
  }
}
</script>
<style lang="less" scoped>
@import 'index1';
</style>
