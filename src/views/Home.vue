<template>
  <div class="home">
    <div id="he-plugin-simple"></div>

    <Header />
    <div class="home-container">
      <div class="content">
        <div class="slogan">
          <!-- title -->
          <div class="title">{{ title }}</div>
          <div class="inputText">{{ inputText }}</div>
          <div class="sub-title">网站每周更新功能, 欢迎收藏关注转发</div>
          <div class="sub-title">
            代码已在github开源
            <a
              href="https://github.com/wangpinpin/devtools-server"
              target="_blank"
              >点击这里</a
            >
          </div>

          <!-- 搜索框 -->
          <div class="search" style="display:none;">
            <input class="searchText" />
            <div class="searchButton">Search</div>
          </div>
        </div>
        <!-- 功能标签 -->
        <div class="card">
          <div
            class="card-tool"
            v-for="(bgf, index) in devBackgroundFamily"
            :key="index"
            :style="bgf"
          >
            <div @click="goTo(devText[index])">{{ devText[index].text }}</div>
          </div>
        </div>
      </div>
    </div>
    <Footer />
    <remote-js
      src="https://widget.heweather.net/simple/static/js/he-simple-common.js?v=1.1"
    ></remote-js>
  </div>
</template>

<script>
// @ is an alias to /src
import Header from "@/components/Header.vue";
import Footer from "@/components/Footer.vue";

export default {
  name: "Home",
  components: {
    Header,
    Footer,
    "remote-js": {
      render(createElement) {
        return createElement("script", {
          attrs: { type: "text/javascript", src: this.src },
        });
      },
      props: {
        src: { type: String, required: true },
      },
    },
  },
  data() {
    return {
      title: "本站已上线",
      inputText: "",
      devBackgroundFamily: [],
      devText: [
        { path: "/DoglickingDiary", text: "舔狗日记" },
        { path: "/MessageBoard", text: "留言板" },
        { path: "/Note", text: "舔狗笔记" },
        {
          path: "/category",
          text: "影音图文",
          children: [
            { path: "/v", text: "看一看" },
            { path: "/Music", text: "听一听" },
            { path: "/wallpaper", text: "必应壁纸" },
            { path: "/EveryDayText", text: "每日一文" },
            { path: "/Wyy", text: "网易云音乐API" },
          ],
        },
        {
          path: "/category",
          text: "摸鱼游戏",
          children: [
            {
              poster: require("@/assets/imgs/games/adarkhome.jpeg"),
              text: "小黑屋",
              path: "/adarkroom",
            },
            {
              poster: require("@/assets/imgs/games/saolei.jpeg"),
              text: "扫雷",
              path: "/saolei",
            },
            {
              poster: require("@/assets/imgs/games/jigsaws.jpg"),
              text: "拼图",
              path: "/jigsaws",
            },
          ],
        },
        {
          path: "/category",
          text: "工具合集",
          children: [
            { path: "/Filetransfer", text: "小破传" },
            { path: "/QRcodeCreate", text: "二维码生成" },
            { path: "/ImageToTxt", text: "图片文字提取" },
            { path: "/JsonFormat", text: "JSON格式化" },
            { path: "/TimeFormat", text: "时间戳转换" },
            { path: "/Base64", text: "图片转Base64" },
            { path: "/ColorTransfer", text: "颜色代码转换" },
            { path: "/PreviewFont", text: "字体预览" },
          ],
        },
        // { path: "/", text: "正在开发" },
      ],
    };
  },
  beforeCreate() {},
  created() {
    this.title = this.title + this.getDay() + "天";

    this.devBackgroundFamily = this.$store.state.devBackgroundFamily.splice(
      0,
      this.devText.length
    );
    this.devBackgroundFamily.sort(function() {
      return Math.random() - 0.5;
    });
    // this.loading.close();
    this.inputAnimation();
  },
  methods: {
    goTo(obj) {
      if (obj.children) {
        this.$store.commit("setCategory", obj.children);
        this.$router.push({
          path: obj.path,
          query: { title: obj.text },
        });
      } else {
        this.$router.push({
          path: obj.path,
        });
      }
    },
    //获取天数
    getDay() {
      let s1 = "2020-08-01";
      s1 = new Date(s1.replace(/-/g, "/"));
      const s2 = new Date();
      const days = s2.getTime() - s1.getTime();
      return parseInt(days / (1000 * 60 * 60 * 24));
    },
    //模拟鼠标光标动画
    inputAnimation() {
      const _this = this;
      setInterval(function() {
        const inputText = "|";
        if (_this.inputText == inputText) {
          _this.inputText = "";
        } else {
          _this.inputText += inputText;
        }
      }, 800);
    },
  },
};
</script>

<style scoped lang="less">
.home-container {
  margin: 0.4rem auto 0;
  text-align: center;
  color: #7c96b1;
  display: flex;

  .content {
    width: 100%;
    .slogan {
      position: relative;
      margin: 0 auto;
      width: 35%;
      .title {
        font-size: 0.48rem;
      }
      .inputText {
        position: absolute;
        right: 1.4rem;
        top: 0.12rem;
        font-size: 0.32rem;
      }
      .sub-title {
        margin-top: 0.2rem;
        font-size: 0.16rem;
        line-height: 0.3rem;
      }
      .search {
        margin: 0.208333rem auto 0;
        display: flex;
        flex-direction: row;
        border: 0.01rem solid #7c96b1;
        width: 58%;
        height: 0.54rem;
        border-radius: 0.5rem;
        line-height: 0.54rem;
        justify-content: space-around;
        .searchText {
          background: none;
          border: none;
          outline: none;
          height: 0.265625rem;
          width: 57%;
          font-size: 0.1rem;
          line-height: 0.26rem;
          color: #7c96b1;
        }
        .searchButton {
          border-left: 0.01rem solid #7c96b1;
          font-size: 0.16rem;
          padding-left: 0.18rem;
          cursor: pointer;
        }
      }
    }
    .card {
      margin: 0.4rem auto 0;
      width: 66%;
      height: 4.8rem;
      display: flex;
      flex-direction: row;
      flex-wrap: wrap;
      color: #fff;
      .card-tool {
        width: 21%;
        height: 1.5rem;
        line-height: 1.5rem;
        border-radius: 0.3rem;
        margin: 0.2rem 2%;
        font-size: 0.21rem;
        cursor: pointer;
        text-align: center;
        a {
          color: #fff;
        }
      }
      .card-tool:hover {
        border: 0.04rem solid #fff;
        box-sizing: border-box;
        line-height: 1.42rem;
      }
    }
  }
}
@media screen and (max-width: 900px) {
  .home {
    height: 90%;
  }
  /deep/#he-plugin-simple {
    display: none;
  }
  .home-container {
    margin-top: 0.2rem;
    height: 88%;
    .content {
      .slogan {
        display: none;
      }
      .card {
        width: 100%;
        justify-content: center;
        align-items: center;
        height: 100%;
        margin: 0;
        overflow-y: scroll;
        .card-tool {
          width: 32%;
          margin: 0.5vh 0.3rem;
          height: auto;
          line-height: 9.5vh;
        }
      }
    }
  }
}
</style>
