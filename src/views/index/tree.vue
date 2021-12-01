<template>
  <div class="tree-container">
    <div class="tree-header">
      <div class="tree-notice-box">
        <div class="tree-notice">
          <van-notice-bar
            background="#ffffff"
            color="#1B0A1C"
            :text="data.notice"
          >
            <template #left-icon>
              <img
                src="@/assets/xinguang/首页 拷贝_slices/公告@2x.png"
                alt=""
              />
            </template>
          </van-notice-bar>
        </div>
      </div>
      <div class="tree-header-swipe">
        <div class="bannerSwp">
          <van-swipe @change="onChange" :autoplay="5000">
            <van-swipe-item v-for="(image, index) in banner" :key="index">
              <a :href="image.url">
                <img
                  style="width: 100%; height: 100%; border-radius: 7px"
                  :src="image.thumb"
                />
              </a>
            </van-swipe-item>
            <div ref="dian" class="custom-indicator" slot="indicator">
              <div
                v-for="(img, x) in banner"
                :key="x"
                :class="x == 0 ? 'selected' : ''"
              ></div>
            </div>
          </van-swipe>
        </div>
      </div>
    </div>

    <div class="tree-list-box">
      <!--<div class="tree-header-swipe-box">-->
      <!--    <video-player  class="video-player vjs-custom-skin" ref="videoPlayer" :playsinline="true" :options="playerOptions"></video-player>-->
      <!--</div>-->
      <img class="top" src="@/assets/xinguang/首页 拷贝_slices/组 1@2x.png" alt="" @click="handleTreeListItemClick(4)">
      <div class="imgs">
        <img src="@/assets/xinguang/首页 拷贝_slices/组 4993@2x.png" alt="" @click="handleTreeListItemClick(9)">
        <img src="@/assets/xinguang/首页 拷贝_slices/组 4993 拷贝@2x.png" alt="" @click="$router.push('share')">
      </div>
      <!-- <div class="tree-list-info-bottom">
        <div
          class="tree-list-info-bottom-back"
          @click="handleTreeListItemClick(12)"
        ></div>
      </div>
      <div class="tree-list-info">
        <div class="tree-list-info-back left-back">
          <div @click="handleTreeListItemClick(4)"></div>
        </div>
        <div class="tree-list-info-back right-back">
          <div @click="handleTreeListItemClick(13)"></div>
        </div>
      </div> -->
    </div>

    <div class="index-menu-list">
      <div class="index-menu-list-item" @click="handleTreeListItemClick(3)">
        <div class="index-menu-list-item-icon one-icon"></div>
        <div class="index-menu-list-item-text">运动加油</div>
      </div>
      <div class="index-menu-list-item" @click="handleTreeListItemClick(6)">
        <div class="index-menu-list-item-icon two-icon"></div>
        <div class="index-menu-list-item-text">积分商城</div>
      </div>
      <div class="index-menu-list-item" @click="handleTreeListItemClick(10)">
        <div class="index-menu-list-item-icon three-icon"></div>
        <div class="index-menu-list-item-text">绑支付宝</div>
      </div>
      <div class="index-menu-list-item" @click="handleTreeListItemClick(11)">
        <div class="index-menu-list-item-icon four-icon"></div>
        <div class="index-menu-list-item-text">关于我们</div>
      </div>
    </div>

    <div class="goods_list">
      <div class="title">商城推荐</div>
      <div class="contents">
        <span class="contents_title">限时积分兑换好礼</span>
        <div class="item" v-for="(item, index) in category_data" :key="index">
          <img :src="item.img" alt="">
          <span class="name">{{ item.title }}</span>
          <div class="info_box">
            <span>{{ item.integral }}</span>
            <div class="btn" @click="category_btn(item.id, dh_num[0][index])">立即兑换</div>
          </div>
        </div>
      </div>
    </div>
    <div class="mask" v-show="ad_show" z-index="999">
      <div class="ops">
        <div class="close">
          <div @click="close()">关闭</div>
        </div>
        <a @click="goAd()">
          <img :src="ad.activity_img" />
        </a>
      </div>
    </div>
  </div>
</template>

<script>
import Fetch from "../../utils/fetch";
// import { Dialog } from 'vant'
// import { videoPlayer } from 'vue-video-player'
import "video.js/dist/video-js.css";
import "vue-video-player/src/custom-theme.css";
export default {
  name: "index",
  // components: { videoPlayer },
  data() {
    return {
      playerOptions: {
        // playbackRates: [0.7, 1.0, 1.5, 2.0], //播放速度
        autoplay: false, //如果true,浏览器准备好时开始回放。
        muted: false, // 默认情况下将会消除任何音频。
        loop: false, // 导致视频一结束就重新开始。
        preload: "auto", // 建议浏览器在<video>加载元素后是否应该开始下载视频数据。auto浏览器选择最佳行为,立即开始加载视频（如果浏览器支持）
        language: "zh-CN",
        aspectRatio: "16:9", // 将播放器置于流畅模式，并在计算播放器的动态大小时使用该值。值应该代表一个比例 - 用冒号分隔的两个数字（例如"16:9"或"4:3"）
        fluid: true, // 当true时，Video.js player将拥有流体大小。换句话说，它将按比例缩放以适应其容器。
        sources: [
          {
            src: "https://rcguanjia.com/mp4/zhounianqing.mp4", // 路径
            type: "video/mp4", // 类型
          },
        ],
        // poster: "https://www.njmbttd.com/upload/dongli.jpg", //你的封面地址
        // width: document.documentElement.clientWidth,
        notSupportedMessage: "此视频暂无法播放，请稍后再试", //允许覆盖Video.js无法播放媒体源时显示的默认信息。
        controlBar: {
          timeDivider: true,
          durationDisplay: true,
          remainingTimeDisplay: false,
          fullscreenToggle: true, //全屏按钮
        },
      },
      footer_type: "n1",
      data: {
        app: {},
        notice: "",
      },
      down_show: true,
      link_width: 0,
      banner: [],
      notice: "",
      links: [],
      type: "n1",
      ad: {},
      ad_show: false,
      examinationData: [
        {
          id: "1",
          name: "one",
          question: "...",
        },
      ],
      indexMenuType: 1,
      category_data: [],
      dh_num: [],
    };
  },
  computed: {
    indexMenuInfoBackClasses() {
      return `info-back-class-${this.indexMenuType}`;
    },
  },
  created() {
    if (this.$parent.getFooterType() === "n1") {
      this.$router.push("/tree").catch((err) => {
        err;
      });
      this.$parent.footer(true, "tree");
    } else {
      this.$parent.footer(true, "tree");
      this.$data.footer_type = "n2";
    }
  },
  mounted() {
    this.start();
    var that = this;
    var bslua = navigator.userAgent;
    if (bslua.indexOf("bsl") != -1) {
      that.down_show = false;
    }
  },
  methods: {
    getIputValue(index) {
      // console.log(index + 1 + "题" + this.radio);
      this.allRadio[index] = this.radio[index]; // 将数据存入提交给后台的数据中
      console.log(this.allRadio);
      // console.log(this.radio)
    },
    toBox(title = "", contain = "") {
      var newsBox = document.getElementsByClassName("modal")[0];
      newsBox.style.display = "block";

      var bodyBox = document.getElementsByClassName("body-box")[0];
      bodyBox.innerHTML = contain;
      var newsTitle = document.getElementsByClassName("news-title")[0];
      newsTitle.innerText = title;
    },
    checkin2() {
      this.$router.push("/qiandao");

      // Fetch('/user/sign').then((res) => {
      // 	if(res.data.coss == 1){
      // 				Dialog.alert({
      // 				    title: '提示',
      // 				    message: res.info,
      // 				    showCancelButton: true,
      // 				    confirmButtonText: "去认证",
      // 				}).then(() => {
      // 				    this.$router.push('/auth');
      // 				}).catch(() => {

      // 				})
      // 	}else{

      // 		// this.sign_money = res.data.reward;
      // 		// this.new_sign_ok = true;
      // 		// this.$notify({
      // 		// 	background: '#07c160',
      // 		// 	message: '签到成功：已获得现金2元和8积分！'
      // 		// });

      // 	}
      // })
    },
    closeBox() {
      var newsBox = document.getElementsByClassName("modal")[0];
      newsBox.style.display = "none";
    },

    appdown_close() {
      this.data.app.is_disable = "N";
    },
    goAd() {
      // this.close();
      // this.$router.push(this.ad.url).catch(err => {err});
      this.close();
      this.$router.push(this.ad.url);
    },
    close() {
      this.ad_show = false;
    },
    category_btn(id, num) {
      Fetch("/index/shop_exchange", {
        gid: id,
        num: num,
      }).then((res) => {
        this.$notify({
          background: "#07c160",
          message: res.info,
        });
      });
    },
    start() {
      setTimeout(() => {
        var element = document.getElementById("app");
        element.scrollIntoView();
      }, 0);
      this.type = this.$parent.getFooterType();
      Fetch("/index/int").then((res) => {
        this.ad = res.data.ad;
        this.data = res.data;

        if (res.data.ad.activity_status == 1) {
          this.ad_show = true;
        } else {
          this.ad_show = false;
        }
      });

      // 兑换信息
      this.category_data = []
      Fetch("/index/goods_list").then((res) => {
        const { category } = res.data
        category.forEach(item => {
          this.category_data = this.category_data.concat(item.list)
        });

        for (var i = 0; i < res.data.category.length; i++) {
          this.dh_num.push([]);
          for (var j = 0; j < res.data.category[i].list.length; j++) {
            this.dh_num[i].push(1);
          }
        }
        console.log('this.category_data', this.category_data)
      });

      /* Fetch('/api/api/image', {type: 'links'}).then(res => {
                    this.links = res.data;
					this.link_width = res.data.length * 23 + (res.data.length-1)*2+"%"
                }); */

      Fetch("/index/banner", {
        type: "banner",
      }).then((res) => {
        this.banner = res.data;
      });
    },
    onChange(index) {
      let els = this.$refs.dian.querySelectorAll("div");
      for (let i = 0; i < els.length; i++) {
        els[i].className = "";
      }
      els[index].className = "selected";
    },
    schedule(schedule) {
      return {
        width: schedule + "%",
      };
    },
    schedule_left(schedule) {
      if (schedule > 80) {
        return {
          left: schedule - 30 + "%",
          color: "#fff",
        };
      } else {
        return {
          left: schedule.slice(0, 5) + "%",
        };
      }
    },
    handleTreeListItemClick(type = 1) {
      switch (type) {
        case 1:
          this.checkin2();

          break;
        case 2:
          this.$router.push({ name: "scratch_card" });
          break;
        case 3:
          this.$router.push({ name: "sport_event" });
          break;
        case 4:
          this.$router.push({ name: "wenjuan" });
          break;
        case 5:
          this.$router.push({ name: "prize" });
          break;
        case 6:
          this.$router.push({ name: "goods" });
          break;
        case 7:
          this.$router.push({ name: "item" });
          break;
        case 8:
          this.$router.push({ name: "baoku" });
          break;
        case 9:
          this.$router.push({ name: "qiandao" });
          break;
        case 10:
          this.$router.push({ name: "alipay" });
          break;
        case 11:
          this.$router.push({ name: "about" });
          break;
        case 12:
          this.$router.push({ name: "auth" });
          break;
        case 13:
          this.$router.push({ name: "alipay" });
          break;
      }
    },
    handleMenuSwitch(type) {
      if (this.indexMenuType === type) return;
      this.indexMenuType = type;
    },
    goToDetail() {
      if (this.indexMenuType == 1) {
        this.handleTreeListItemClick(9);
      }
      if (this.indexMenuType == 2) {
        this.$router.push("/question_type");
      }
      if (this.indexMenuType == 3) {
        this.$router.push("/share");
      }
      if (this.indexMenuType == 4) {
        this.$router.push("/alipay");
      }
    },
  },
};
</script>

<style lang="less" scoped>
@import "tree";
</style>
