<template>
  <page class="mainHome">
    <div slot="content">

      <section v-bind:style="{'margin-bottom':isMobile?'-18px':''}" class="platfrom-header heard_card primary_mg primary_bg primary_flex_center fn-17">
        <div @click="showLocation" class="index_left">
          <img class="icon-18" src="static/img/icon/location.png" />
          <span class="fn-16">闵行区浦江第一中学(浦锦路西50米)</span>
          <img class="icon-10" src="static/img/icon/down-w.png" />
        </div>
        <aside class="index_right">
          <div>
            <h2>20.2°</h2>
            <p>晴夜</p>
          </div>
          <!-- <lazy-image :src="'static/img/dark.png'" :placeholder="loaddingImg" /> -->
          <img src="static/img/dark.png" alt="">
        </aside>
      </section>

      <!-- 搜索商家，商家名称 -->
			<section v-bind:style="{height:isMobile?'6rem':'4rem'}" class="primary_bg float-header input_box primary_flex_center search primary_mg" style="z-index:10;top:0px">
      <!-- <section v-bind:style="{height:isMobile?'6rem':'4rem'}" class=" float-header input_box primary_flex_center search primary_mg" > -->
        <div @click="showSearch" class="ipt_item primary_flex_center fn-12">
          <img class="icon-10" src="static/img/icon/search.png" />&nbsp;&nbsp;搜索商家，商家名称
        </div>
      </section>

      <!-- 小内容滚动区域 -->
      <section class="primary_bg primary_mg_hor" style="height:0px;overflow:hidden;height:35px;">
        <div style="overflow-x: scroll;white-space: nowrap;position:relative;height:45px">
          <div class="input_box primary_flex_center scrllX fn-13" style="padding:0px;height:35px;">
            <a v-for="item in tipList" :key="item">{{item}}</a>
          </div>
        </div>
      </section>

			<!-- 商家分裂 内容item区域 -->
      <section class="primary_bg_white" style="height:175px;">
        <mt-swipe :auto="0" height="100%">
          <mt-swipe-item v-for="(item,index) in menuList" :key="index">
            <div class="row">
              <div class="col col_item" v-for="(it,index1) in item" :key="index1">
                <div>
                  <lazy-image :src="it.img" :img-class="['detailImg']" :placeholder="loaddingImg" />
                </div>
                <span>{{it.text}}</span>
              </div>
            </div>
          </mt-swipe-item>
        </mt-swipe>
      </section>

      <!-- 商家广告区域 -->
      <shopAds></shopAds>

      <!-- 商家列表 -->
      <section class="shopList primary_bg_white">
        <div class="primary_mg">
          <span class="title ">推荐商家</span>
        </div>
        <div>
					<!-- 单个商家展示组件 -->
          <shopItem v-for="(item,index) in shopList" :key="index"></shopItem>
        </div>
      </section>
      <section class="footer_line fn-13">我是有底线了</section>
    </div>
  </page>
</template>
<script type="text/babel">
import menuList from "../../assets/data/homeItem";
export default {
  name: "MainCom",
  data() {
    return {
      menuList: menuList,
      topStatus: "",
      wrapperHeight: 0,
      translate: 0,
      moveTranslate: 0,
      tipList: ["烧烤", "馒头", "麻辣烫", "粥", "面包", "芒果", "拉面", "酸辣粉", "冒菜", "蛋糕"],
      shopList: [1, 1, 1, 2, 3, 4, 5, 6]
    };
  },
  components: {
		//单个商品显示
    ShopItem: r => {
      require.ensure([], () => r(require("./components/ShopItem")), "ShopItem");
		},
		//商品广告
    ShopAds: r => {
      require.ensure([], () => r(require("./components/ShopAds")), "ShopAds");
    }
  },
  methods: {
    /**
     * 显示搜索框
     */
    showSearch() {
      this.$store.commit("POP_STATUS_S", true);
    },
    /**
     * 跳转商家列表
     */
    toShopDetail(item) {
      console.log("商家列表", item);
      this.$router.push({ name: "shopDetail" });
    },
    /**
    * 显示地址
    */
    showLocation() {
      this.$store.commit("POP_STATUS_L", true);
    }
  }
};
</script>
<style lang='scss'>
.mainHome {
  .ivu-affix {
    z-index: 2;
  }
  .mint-swipe-indicators {
    .mint-swipe-indicator {
      width: 10px;
      height: 2px;
      border-radius: 1px;
      opacity: 1;
      background: gray;
    }
    .is-active {
      background: black;
    }
  }
  font-weight: 200;
  .youhui {
    width: 100%; // background: url('static/img/common/youhui.png');
    // padding: 0px 10px;
    img {
      width: calc(100% - 20px);
      margin-left: 10px;
    }
  }
  .col_item {
    min-height: 80px;
    text-align: center;
    div {
      padding-top: 5px;
      display: flex;
      justify-content: center;
      img {
        width: 4.5rem;
        height: 4.5rem;
        display: block;
      }
    }
    span {
      text-align: center;
      color: rgb(102, 102, 102);
    }
  }
  .scrllX {
    justify-content: left !important;
    position: absolute;
    z-index: 1;
    height: 50px;
    a {
      text-align: left;
      font-family: -webkit-body;
      color: white;
      margin-right: 2rem;
    }
  }
  .search {
    position: sticky;
    top: 0px;
    ::after {
      content: "";
      position: absolute;
      width: 100%;
      height: 0px;
      left: 0;
      background-image: linear-gradient(
        90deg,
        rgb(0, 170, 255),
        rgb(0, 133, 255)
      );
    }
  }
  .input_box {
    height: 4.5rem;
    text-align: center;
    justify-content: center;
    color: gray;
    .ipt_item {
      color: black;
      text-align: center;
      background: rgb(255, 255, 255);
      width: 100%;
      min-height: 3rem;
      height: 3rem; // line-height: 39px;
      justify-content: center;
      border-radius: 2px;
    }
  }
  .heard_card {
    height: auto;
    color: rgb(255, 255, 255);
    justify-content: space-between;
    padding-bottom: 0px;
    position: relative;
    z-index: 200;
    .index_left {
      align-items: center;
      height: 4rem;
      display: flex;
      -webkit-box-align: center;
      width: 60%;
      padding-top: 10px;
      span {
        font-weight: 600;
        margin: 0 5px;
        max-width: 80%;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
      }
    }

    .index_right {
      // width: 40%;
      display: flex;
      align-items: center;
      text-align: right;
      padding-top: 10px;
      img {
        height: 28px;
        margin: 0px 5px 0px 5px;
      }
      div {
        h2,
        p {
          font-size: 1rem;
          margin: 0px;
          font-weight: 400;
        }
      }
    }
  }
}
</style>
