<template>
  <div class="register-container">
    <div class="register-box">
      <!-- <div class="item" style="display:none;">
					<div class="left">国家/地区</div>
					<div class="right" @click="handleSelectArea">
						<span>{{data.telAddress}}</span>
						<van-icon name="arrow" size="18" color="#A8A8A8"/>
					</div>
				</div> -->
      <img
        class="back"
        src="@/assets/xinguang/注册_slices/矢量智能对象@2x(1).png"
        alt=""
      />
      <img
        class="back2"
        src="@/assets/xinguang/注册_slices/矢量智能对象@2x.png"
        alt=""
      />
      <div class="title">
        <span @click="handleLogin">登陆</span>
        <span>手机号注册</span>
      </div>

      <div class="item">
        <img src="@/assets/xinguang/登录_slices/24gf-phoneBubble@2x.png" />
        <input type="number" v-model="data.mobile" placeholder="请输入手机号" />
      </div>
      <div class="item">
        <img src="@/assets/xinguang/登录_slices/密码（开） (1)@2x.png" />
        <input
          type="password"
          v-model="data.password"
          placeholder="密码为6-16位字母和数字组合"
        />
      </div>
      <div class="item">
        <img src="@/assets/xinguang/登录_slices/密码（开） (1)@2x.png" />
        <input
          type="password"
          v-model="data.spassword"
          placeholder="请再次输入"
        />
      </div>
      <div class="item">
        <img src="@/assets/xinguang/注册_slices/验证码 (1)@2x.png" />
        <input v-model="data.code" placeholder="请输入验证码" />
        <div class="code-btn" @click="sendcode()">
          {{ timeData.seconds > 0 ? timeData.seconds : "发送验证码" }}
        </div>
      </div>
      <div class="item">
        <img src="@/assets/xinguang/注册_slices/邀请码 (1)@2x.png" />
        <input v-model="data.t_mobile" placeholder="请输入邀请码(可不填)" />
      </div>

      <div class="register-btn" @click="handleSubmit">注 册</div>
    </div>
    <div class="captcha" id="captcha" v-show="!showCaptcha"></div>

    <div class="login-footer">
      <div class="imgs">
        <img src="@/assets/xinguang/登录_slices/椭圆 1@2x.png" alt="" />
        <img src="@/assets/xinguang/登录_slices/勾 (1)@2x.png" alt="" />
      </div>

      <div>我已阅读并接受</div>
      <div class="footer-link" @click="handleGoUserTerms">用户协议</div>
      <div>与</div>
      <div class="footer-link" @click="handleGoPrivacyPolicy">隐私政策</div>
    </div>

    <!-- <div class="register-form-box">
				<form class="form" @submit.prevent="handleSubmit">
					<van-field v-model="data.telAddress" label-width="1.7rem" label="国家/地区" readonly @click-right-icon="handleSelectArea" input-align="right" right-icon="arrow" />
					<van-field v-model="data.mobile" label-width="1.7rem" :label="formAreaLabel" type="tel" placeholder="请输入您的手机号码" />
					<van-field v-model="data.password" label-width="1.7rem" label="登陆密码" type="password" placeholder="请输入登陆密码" />
					<van-field v-model="data.spassword" label-width="1.7rem" label="确认密码" type="password" placeholder="请再次输入密码" />
					<van-field v-model="data.code" label-width="1.7rem" center clearable label="验证码" placeholder="请输入验证码">
						<template #button>
							<div class="code-btn" @click="sendcode()">{{timeData.seconds > 0 ? timeData.seconds : '获取验证码'}}</div>
						</template>
					</van-field>
					<van-field v-model="data.t_mobile" label-width="1.7rem" label="邀请码" placeholder="请输入邀请码（可不填）" />
					<div class="register-form-item">
						<div @click="handleLogin()">已有账号？登陆</div>
					</div>
					<div class="register-form-btn" @click="handleSubmit">立即注册</div>
					<div class="register-form-footer">
						<div class="footer-checkbox"><van-checkbox v-model="loginTerms" shape="square"></van-checkbox></div>
						<div>我已阅读并接受</div>
						<div class="footer-link" @click="handleGoUserTerms">条款及细则</div>
						<div>与</div>
						<div class="footer-link" @click="handleGoPrivacyPolicy">客户隐私政策</div>
					</div>
				</form>
			</div> -->
    <!-- </div> -->
    <van-action-sheet
      v-model="isShowAction"
      :actions="actionsList"
      cancel-text="取消"
      close-on-click-action
      @select="handleSelectAction"
      @cancel="handleCancelAction"
    />
  </div>
</template>
<script src="//g.alicdn.com/mtb/lib-flexible/0.3.4/??flexible_css.js,flexible.js"></script>
<script>
import Vue from "vue";
import qs from "qs";
import md5 from "js-md5";
import { CountDown, Checkbox } from "vant";
import Fetch from "../../utils/fetch";

Vue.use(CountDown).use(Checkbox);

export default {
  name: "index",
  data() {
    return {
      showCaptcha: true,
      time: 0,
      bg: "",
      data: {
        telAddress: "中国大陆（+86）",
        mobile: "",
        password: "",
        spassword: "",
        code: "",
        t_mobile: "",
      },
      base64Image: "",
      is_t_inp: false,
      is_send: false,
      checked: true,
      config: {},
      // 用户隐私条款
      loginTerms: false,
      timeData: {
        seconds: 0,
      },
      isShowAction: false,
      formAreaLabel: "+86",
      actionsList: [
        { name: "中国大陆（+86）", labelName: "+86", type: 1 },
        { name: "中国澳门（+853）", labelName: "+853", type: 2 },
        { name: "中国香港（+852）", labelName: "+852", type: 3 },
        { name: "中国台湾（+886）", labelName: "+886", type: 4 },
      ],
    };
  },
  created() {
    this.$parent.footer(false);
    this.set_ALY_config().then((res) => {
      this.get_ALY_file();
    });
  },
  mounted() {
    const params = location.search.replace("?", "").replace("/", "");
    var query = qs.parse(params);
    /* this.data.t_mobile = query.m;
			if (query.m) {
			    this.is_t_inp = true;
			} */
    this.data.t_mobile = this.$route.query.m;
    if (this.$route.query.m) {
      this.is_t_inp = true;
    }

    this.start();
    window.returnIosIdfa = this.returnIosIdfa;
    // this.getImageCode();
  },
  methods: {
    // getImageCode() {
    //     Fetch("/api/api/imageCode").then(res => {
    //         this.base64Image = res.data.code
    //     });
    // },
    start() {
      Fetch("/index/webconfig", {
        type: "bg",
      }).then((res) => {
        this.bg = 'url("' + res.data.reg + '")';
      });
      Fetch("/index/webconfig", {
        type: "web",
      }).then((res) => {
        this.config = res.data;
      });
    },
    clear() {
      this.data.username = "";
    },
    timeCall() {
      this.is_send = false;
      this.time = 0;
    },
    sendcode() {
      if (this.is_send) {
        return;
      }

      if (!this.config.sms_verify) {
        return;
      }

      if (!this.data.mobile) {
        this.$notify("请输入手机号");
        return;
      }

      // if (!this.data.image_code) {
      //     this.$notify("请输入图片验证码");
      //     return;
      // }

      this.is_send = true;

      Fetch("/index/register_code", {
        mobile: this.data.mobile,
      })
        .then(() => {
          this.time = 60 * 1000;
          this.$notify({
            background: "#07c160",
            message: "发送成功",
          });
        })
        .catch(() => {
          this.is_send = false;
        });
    },
    handleSubmit() {
      if (!this.data.mobile) {
        this.$notify("请输入您的手机号");
        return;
      }

      if (!this.data.password) {
        this.$notify("请输入登录密码");
        return;
      }

      if (this.data.password !== this.data.spassword) {
        this.$notify("两次输入的密码不一至");
        return;
      }

      if (!this.data.code && this.config.sms_verify) {
        this.$notify("请输入验证码");
        return;
      }

      if (!this.checked) {
        this.$notify("请同意用户协议");
        return;
      }

      if (/LT-APP/.test(navigator.userAgent)) {
        this.data.mobile_system = 2;
      } else {
        try {
          var ASIdentifierManager = plus.ios.importClass("ASIdentifierManager");
          var sharedManager = ASIdentifierManager.sharedManager();
          var idfa = sharedManager.advertisingIdentifier().UUIDString();
          var iuid = md5(idfa.toUpperCase()).toLowerCase();
          this.data.mobile_system = 1;
        } catch (e) {
          //TODO handle the exception
          var u = navigator.userAgent;
          if (u.indexOf("Android") > -1 || u.indexOf("Linux") > -1) {
            this.data.mobile_system = 2;
          } else if (u.indexOf("iPhone") > -1) {
            this.data.mobile_system = 1;
          }
        }
      }
      this.toreg(getNVCVal());
    },
    getIosIdfa() {
      if (typeof window.webkit == "undefined") {
        //进入下载页
        this.$router.push({ path: `/down` });
        return;
      }
      window.webkit.messageHandlers.idfa.postMessage({ result: "idfa" }); //app中接收到之后会回调给下面的函数
    },
    returnIosIdfa(idfa) {
      Fetch("/index/check", {
        os: 1,
        imei: idfa,
        type: 2,
      });
      this.$router.replace("/tree");
    },
    // 动态加载js文件
    get_ALY_file() {
      try {
        var stamp = new Date().getTime();
        var script = document.createElement("script");
        script.setAttribute("type", "text/javascript");
        script.setAttribute(
          "src",
          "https://g.alicdn.com/sd/nvc/1.1.112/guide.js?t=" + stamp
        );
        document.documentElement.appendChild(script);
      } catch (e) {}
    },
    set_ALY_config() {
      var _this = this;
      return new Promise((resolve, reject) => {
        window.NVC_Opt = {
          //无痕配置 && 滑动验证、刮刮卡、问答验证码通用配置
          appkey: "CF_APP_1",
          // scene: "nvc_register_h5",
          appkey: "FFFF0N0000000000A5C6",
          scene: "nvc_register_h5",
          isH5: true,
          popUp: false,
          renderTo: "#captcha",
          nvcCallback: function (data) {
            // console.log(data);
            _this.toreg(data);
            // data为getNVCVal()的值，此函数为二次验证滑动或者刮刮卡通过后的回调函数
            // data跟业务请求一起上传，由后端请求AnalyzeNvc接口，接口会返回100或者900
          },
          trans: { key1: "code0", nvcCode: 400 },
          language: "cn",
          //滑动验证长度配置
          customWidth: 300,
          //刮刮卡配置项
          width: 300,
          height: 100,
          elements: [
            "//img.alicdn.com/tfs/TB17cwllsLJ8KJjy0FnXXcFDpXa-50-74.png",
            "//img.alicdn.com/tfs/TB17cwllsLJ8KJjy0FnXXcFDpXa-50-74.png",
          ],
          bg_back_prepared:
            "//img.alicdn.com/tps/TB1skE5SFXXXXb3XXXXXXXXXXXX-100-80.png",
          bg_front:
            "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAABQCAMAAADY1yDdAAAABGdBTUEAALGPC/xhBQAAAAFzUkdCAK7OHOkAAAADUExURefk5w+ruswAAAAfSURBVFjD7cExAQAAAMKg9U9tCU+gAAAAAAAAAIC3AR+QAAFPlUGoAAAAAElFTkSuQmCC",
          obj_ok: "//img.alicdn.com/tfs/TB1rmyTltfJ8KJjy0FeXXXKEXXa-50-74.png",
          bg_back_pass:
            "//img.alicdn.com/tfs/TB1KDxCSVXXXXasXFXXXXXXXXXX-100-80.png",
          obj_error:
            "//img.alicdn.com/tfs/TB1q9yTltfJ8KJjy0FeXXXKEXXa-50-74.png",
          bg_back_fail:
            "//img.alicdn.com/tfs/TB1w2oOSFXXXXb4XpXXXXXXXXXX-100-80.png",
          upLang: {
            cn: {
              _ggk_guide: "请在屏幕上滑动，刮出两面盾牌",
              _ggk_success: "恭喜您成功刮出盾牌<br/>继续下一步操作吧",
              _ggk_loading: "加载中",
              _ggk_fail: [
                "呀，盾牌不见了<br/>请",
                "javascript:NoCaptcha.reset()",
                "再来一次",
                "或",
                "http://survey.taobao.com/survey/QgzQDdDd?token=%TOKEN",
                "反馈问题",
              ],
              _ggk_action_timeout: [
                "我等得太久啦<br/>请",
                "javascript:NoCaptcha.reset()",
                "再来一次",
                "或",
                "http://survey.taobao.com/survey/QgzQDdDd?token=%TOKEN",
                "反馈问题",
              ],
              _ggk_net_err: [
                "网络实在不给力<br/>请",
                "javascript:NoCaptcha.reset()",
                "再来一次",
                "或",
                "http://survey.taobao.com/survey/QgzQDdDd?token=%TOKEN",
                "反馈问题",
              ],
              _ggk_too_fast: [
                "您刮得太快啦<br/>请",
                "javascript:NoCaptcha.reset()",
                "再来一次",
                "或",
                "http://survey.taobao.com/survey/QgzQDdDd?token=%TOKEN",
                "反馈问题",
              ],
            },
          },
        };
        resolve();
      });
    },
    toreg(nvc) {
      let _this = this;
      this.data.nvc = nvc;
      Fetch("/index/newregister", {
        ...this.data,
      }).then((res) => {
        console.log(res.data.BizCode);
        if (res.data.BizCode == 400) {
          //唤醒滑动验证
          _this.showCaptcha = false;
          getNC().then(function () {
            NoCaptcha.upLang("cn", {
              LOADING: "加载中...", //加载
              SLIDER_LABEL: "请向右滑动验证", //等待滑动
              CHECK_Y: "验证通过", //通过
              ERROR_TITLE: "非常抱歉，这出错了...", //拦截
              CHECK_N: "验证未通过", //准备唤醒二次验证
              OVERLAY_INFORM: "经检测你当前操作环境存在风险，请输入验证码", //二次验证
              TIPS_TITLE: "验证码错误，请重新输入", //验证码输错时的提示
            });
            _nvc_nc.reset();
          });
        } else if (res.data.BizCode == 100 || res.data.BizCode == 200) {
          //登录成功
          nvcReset();
          _this.showCaptcha = true;
          if (res.data.token) {
            localStorage.setItem("token", res.data.token);
          }
          this.$notify({
            background: "#07c160",
            message: "注册成功",
          });
          this.$router.replace("/tree");
          let that = this;
          // 安卓原生
          if (/LT-APP/.test(navigator.userAgent)) {
            jsBridge.getOAID(function (oaid) {
              if (oaid) {
                // alert(oaid);
                // that.$notify('oaid:'+oaid);
                Fetch("/index/check", {
                  oaid: oaid,
                  imei: "",
                  os: 0,
                  type: 2,
                }).then((res) => {
                  this.$router.replace("/tree");
                });
              } else {
                jsBridge.getIMEI(function (imei) {
                  // that.$notify('imei:'+imei);
                  Fetch("/index/check", {
                    imei: imei,
                    os: 0,
                    type: 2,
                  }).then((res) => {
                    this.$router.replace("/tree");
                  });
                });
              }
            });
            this.$router.replace("/tree");
          } else {
            // ios原生
            this.getIosIdfa();
          }
        } else if (res.data.BizCode == 800 || res.data.BizCode == 900) {
          //直接拦截
          nvcReset();
          _this.$notify({
            background: "#07c160",
            message: "注册失败，请重新注册。",
          });
        }
      });
    },
    // 登录
    handleLogin() {
      this.$router.push({ name: "login" });
    },
    // 隐私协议
    handleGoPrivacyPolicy() {
      this.$router.push({ path: this.config.user_contract_ys_link });
    },
    // 用户条款
    handleGoUserTerms() {
      this.$router.push({ path: this.config.user_contract_link });
    },
    // 选择地区
    handleSelectArea() {
      this.isShowAction = true;
    },
    handleSelectAction(item) {
      this.formAreaLabel = item.labelName;
      this.data = Object.assign({}, this.data, {
        telAddress: item.name,
      });
    },
    handleCancelAction() {
      this.isShowAction = false;
    },
    handleBack() {
      this.$router.push({ name: "login" });
    },
  },
};
</script>

<style lang="less" scoped>
@import "register";
</style>
