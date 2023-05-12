<template>
  <view class="page">
    <view class="my-top">
      <!-- head -->
      <view
        class="head"
        :style="'background-color: rgba(255,255,255,' + scrollTop / 50 + ');'"
      >
        <view class="portrait">
          <view class="setting-mess">
            <view class="setting" @click="onCollect('gohome')">
              <text
                class="iconfont icon-home"
                :style="scrollTop > 20 ? 'color:#333333' : ''"
              ></text>
            </view>
          </view>
        </view>
        <view class="title">
          <text v-show="scrollTop > 20"></text>
        </view>
        <view class="setting-mess">
          <view class="setting" @click="onSetting">
            <text
              class="iconfont icon-setting"
              :style="scrollTop > 20 ? 'color:#333333' : ''"
            ></text>
          </view>
         <!-- <view class="mess" @click="kefu">
            <text
              class="iconfont icon-xiaoxi"
              :style="scrollTop > 20 ? 'color:#333333' : ''"
            ></text>
          </view> -->
        </view>
      </view>
      <!-- 用户信息 -->
      <view class="user-info">
        <view class="portrait">
          <image
            :src="userinfo.headPortrait"
          ></image>
        </view>
        <view class="info">
          <view class="nickname">
            <text>{{userinfo.customName}}  </text>
          </view>
          <view class="nickname">
            <text>{{userinfo.customSex=='1'?'男':'女'}}</text> <text>{{userinfo.customPhoneNum}}</text>
          </view>
        </view>
      </view>
      <!-- <view class="user-info" @click="onUserInfo">
				<view class="portrait">
				  <image src="http://img2.imgtn.bdimg.com/it/u=1039075865,3371165857&fm=26&gp=0.jpg"></image>
				</view>
				<view class="info">
				  <view class="nickname">
					<text>登录/注册</text>
				  </view>
				</view>
			  </view> -->

      <!-- 会员 -->
      <view class="vip-info" @click="kefu">
        <view class="vip">
          <text v-if="userinfo.customLevel == 0">普通会员</text>
          <text v-if="userinfo.customLevel == 1">黄金会员</text>
          <text v-if="userinfo.customLevel == 2">铂金会员</text>
          <text class="line"></text>
        </view>
        <view class="vip-explain">
          <text>联系客服获得优惠方案</text>
        </view>
        <view class="vip-btn">
          <text @click="kefu">官方客服</text>
        </view>
      </view>
    </view>
    <!-- 订单信息 -->
    <view class="order-info">
      <view class="list" @click="onSkipOrder(0)">
        <view class="icon">
          <text class="iconfont icon-daifukuan"></text>
          <!-- <text class="num">22</text> -->
        </view>
        <view class="title">
          <text>全部</text>
        </view>
      </view>
      <view class="list" @click="onSkipOrder(1)">
        <view class="icon">
          <text class="iconfont icon-daifahuo"></text>
          <!-- <text class="num">22</text> -->
        </view>
        <view class="title">
          <text>待发货</text>
        </view>
      </view>
      <view class="list" @click="onSkipOrder(2)">
        <view class="icon">
          <text class="iconfont icon-daishouhuo"></text>
          <!-- <text class="num">22</text> -->
        </view>
        <view class="title">
          <text>已完成</text>
        </view>
      </view>
      <!-- <view class="list" @click="onSkipOrder(4)">
				<view class="icon">
					<text class="iconfont icon-daipingjia"></text>

				</view>
				<view class="title">
					<text>待评价</text>
				</view>
			</view>
			<view class="list" @click="onSkipOrder(5)">
				<view class="icon">
					<text class="iconfont icon-tuikuan"></text>

				</view>
				<view class="title">
					<text>提现</text>
				</view>
			</view> -->
    </view>
    <!-- 钱包 -->
    <view class="wallet-info">
      <view class="list" @click="onWallet('chongzhi')">
        <view class="icon">
          <text class="iconfont icon-tuikuan"></text>
        </view>
        <view class="title">
          <text>充值</text>
        </view>
      </view>
      <view class="list" @click="onWallet('tixian')">
        <view class="icon">
          <text class="iconfont icon-qianbao"></text>
        </view>
        <view class="title">
          <text>提现</text>
        </view>
      </view>
      <view class="list" @click="onWallet('wallet')">
      <!-- <view class="list" > -->
        <view class="icon">
          <text class="number">{{userinfo.givenAccount}}</text>
        </view>
        <view class="title">
          <text>余额</text>
        </view>
      </view>
    </view>
    <!-- 积分，付款码 -->
    <view class="integral-payment">
      <view class="list" @click="onSetting">
        <view class="title">
          <text
            class="cuIcon-card"
            style="font-weight: bold; width: 50rpx; font-size: 20px"
          ></text>
          <text>个人资料</text>
        </view>
        <view class="mess">
          <text>填写信息 更安全</text>
        </view>
      </view>
      <view class="list" @click="onWallet('SignIn')">
        <view class="title">
          <text
            class="cuIcon-card"
            style="font-weight: bold; width: 50rpx; font-size: 20px"
          ></text>
          <text>银行卡</text>
        </view>
        <view class="mess">
          <text>绑定银行卡 更便捷</text>
        </view>
      </view>
    </view>
    <!-- 我的服务 -->
    <view class="my-service">
      <view class="title">
        <text>我的服务</text>
      </view>
      <view class="service-list">
        <!-- <view class="list" @click="onServer('feedback')"> -->
		<view class="list">
          <view class="thumb">
            <text class="cuIcon-edit" style="font-size: 20px"></text>
          </view>
          <view class="name">
            <text>关于Naturecan</text>
          </view>
        </view>
        <view class="list" @click="kefu2()">
          <view class="thumb">
            <!-- <image src="/static/kfrx.png"></image> -->
            <text class="cuIcon-people" style="font-size: 20px"></text>
          </view>
          <view class="name">
            <text>客服热线</text>
          </view>
        </view>
        <view class="list" >
          <view class="thumb">
            <text class="cuIcon-exit" style="font-size: 20px"></text>
            <!-- <svg width="24" height="24" viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg"><rect width="30" height="30" fill="white" fill-opacity="0.01"/><path d="M23.9917 6L6 6L6 42H24" stroke="#333" stroke-width="4" stroke-linecap="round" stroke-linejoin="round"/><path d="M33 33L42 24L33 15" stroke="#333" stroke-width="4" stroke-linecap="round" stroke-linejoin="round"/><path d="M16 23.9917H42" stroke="#333" stroke-width="4" stroke-linecap="round" stroke-linejoin="round"/></svg> -->
          </view>
          <view class="name" @click="loginout">
            <text>退出登录</text>
          </view>
        </view>
      </view>
    </view>
	<!-- 我的服务 -->
	<view class="my-service">
	  <view class="title">
	    <text>关于平台</text>
	  </view>
	  <!-- <view class="service-list" style="color: #959595;text-align: center;padding-bottom: 20rpx;">
	    Naturecan于2005年在英国成立，2011年登陆美国，2015年来到中国，并相继在印度、俄罗斯及德国等国家开设分站。多年来，Naturecan获得了令人瞩目的成绩。
		<img src="../../static/styleimg/team.jpeg" alt="">
	  </view> -->
	  <view class="service-list" style="color: #959595;text-align: center;padding-bottom: 20rpx;">
	    Naturecan商城于1996年美国成立，2010年来到中国，向全世界150多个国家提供商品。多年来，Naturecan获得了令人曙目的成绩。

	  </view>
	</view>
    <!-- 为你推荐 -->
    <!-- <view class="recommend-info">
      <view class="recommend-title">
        <view class="title">
          <image src="/static/wntj_title.png" mode=""></image>
        </view>
      </view>
      <view class="goods-list">
        <view
          class="list"
          v-for="(item, index) in goodsList"
          @click="onSkip('goods')"
          :key="index"
        >
          <view class="pictrue">
            <image :src="item.img" mode="heightFix"></image>
          </view>
          <view class="title-tag">
            <view class="tag">
              <text v-if="item.is_goods === 1">特价</text>
              {{ item.name }}
            </view>
          </view>
          <view class="price-info">
            <view class="user-price">
              <text class="min">￥</text>
              <text class="max">{{ item.price }}</text>
            </view>
            <view class="vip-price">
              <image src="/static/vip_ico.png"></image>
              <text>￥{{ item.vip_price }}</text>
            </view>
          </view>
        </view>
      </view>
    </view> -->
    <!-- 客服热线 -->
	<view style="text-align: center;">
		<view style="">
			<img style="width: 80px;margin: 0 30rpx;" src="../../static/styleimg/footer-badge-satisfaction.png" alt="">
			<img style="width: 80px;margin: 0 30rpx;" src="../../static/styleimg/footer-badge-years.png" alt="">
		</view>
		<view style="clear: both;"></view>
	</view>
	<view style="text-align: center;font-size: 10rpx;padding: 20px 0 50px;">© 2005 - 2022 TopCashback - 版权所有</view>
	<view style="text-align: center;">
		<view style="">
			<img style="width: 120px;" src="../../static/styleimg/footer-ssl-cn.png" alt="">
		</view>
		<view style="clear: both;"></view>
	</view>
    <!-- tabbar -->
	<DialogBox ref="DialogBox"></DialogBox>
    <!-- <TabBar :tabBarShow="3"></TabBar> -->
  </view>
</template>

<script>
import { updateCustomData,findCustomData,findKefu } from "../../api/index";
import TabBar from "../../components/TabBar/TabBar.vue";
	import __config from "../../config/env.js";
export default {
  components: {
    TabBar,
  },
  data() {
    return {
		action:__config.imgaction,
      scrollTop: 0,
      isHotline: false,
      goodsList: [],
	  userinfo:[]
    };
  },
  onReady() {
    uni.hideTabBar();
  },
  onPageScroll(e) {
    this.scrollTop = e.scrollTop;
  },
  onLoad() {
  	this.checkuserinfo()
  },
  onShow() {
  	this.checkuserinfo()
  },
  methods: {
	  loginout(){
		  this.$refs['DialogBox'].confirm({
		  	title: '提示',
		  	content: '是否要退出登录?',
		  	DialogType: 'inquiry',
		  	animation: 0
		  }).then(()=>{
		  	uni.clearStorage();
		  	uni.reLaunch({
		  		url:'/pages/login/login'
		  	})
		  	
		  
		  })
	  },
	  checkuserinfo(){
	  	let _this = this
	  	
	  	findCustomData().then(res=>{
	  		_this.userinfo = res.result
			
			_this.userinfo.headPortrait = _this.action+'/'+res.result.headPortrait

			
	  	})
	  
	  },
    //客服
    kefu() {
		findKefu().then((res)=>{
			console.log(res.result)
			window.location.href="https://chatlink.mstatik.com/widget/standalone.html?eid="+res.result
		})
	},
	kefu2() {
		
		uni.makePhoneCall({
			phoneNumber: '(+1) 9792707575' //仅为示例
		});
	},
    /**
     * 关注跳转
     */
    onCollect(type) {
      switch (type) {
        case "gohome":
          uni.navigateTo({
            url: "/pages/home/home",
          });
          break;
        case "content":
          uni.navigateTo({
            url: "/pages/ContentCollection/ContentCollection",
          });
          break;
        case "record":
          uni.navigateTo({
            url: "/pages/BrowsingHistory/BrowsingHistory",
          });
          break;
      }
    },
    /**
     * 订单
     */
    onSkipOrder(type) {
      if (type === 5) {
        uni.navigateTo({
          url: "/pages/AfterSalesOrder/AfterSalesOrder",
        });
        return;
      }
      uni.navigateTo({
        url: "/pages/MyOrderList/MyOrderList?type=" + type,
      });
    },
    /**
     * 钱包跳转点击
     */
    onWallet(type) {
      switch (type) {
		
        case "integral":
          uni.navigateTo({
            url: "/pages/IntegralDetails/IntegralDetails",
          });
          break;
        case "coupon":
          uni.navigateTo({
            url: "/pages/MyCoupon/MyCoupon",
          });
          break;
        case "wallet":
          uni.navigateTo({
            url: "/pages/ConsumeRecord/ConsumeRecord?type=0",
          });
          break;
        case "SignIn":
          uni.navigateTo({
            url: "/pages/SignIn/SignIn",
          });
          break;
        case "payment":
          uni.navigateTo({
            url: "/pages/PaymentCode/PaymentCode",
          });
          break;
		  case "chongzhi":
		      uni.navigateTo({
		        url: "/pages/MyWallet/MyWallet",
		      });
		      break;
		  case "tixian":
		  	  uni.navigateTo({
		  	    url: "/pages/cart/cart",
		  	  });
		  	  break;
      }
    },
    /**
     * 我的服务点击
     */
    onServer(type) {
      switch (type) {
        case "feedback":
          uni.navigateTo({
            url: "/pages/Feedback/Feedback",
          });
          break;
        case "serve":
          this.isHotline = true;
          break;
      }
    },
    /**
     * 设置点击
     */
    onSetting() {
      uni.navigateTo({
        url: "/pages/Setting/Setting",
      });
    },
    /**
     * 消息点击
     */
    onMessage() {
      uni.navigateTo({
        url: "/pages/Message/Message",
      });
    },
    /**
     * 会员点击
     */
    onMmeberVip() {
      uni.navigateTo({
        url: "/pages/MembersOpened/MembersOpened",
      });
    },
    /**
     * 跳转点击
     * @param {String} type 跳转类型
     */
    onSkip(type) {
      switch (type) {
        case "goods":
          uni.navigateTo({
            url: "/pages/GoodsDetails/GoodsDetails",
            animationType: "zoom-fade-out",
            animationDuration: 200,
          });
          break;
      }
    },
    /**
     * 用户信息点击
     * @param {Number} type
     */
    onUserInfo() {
      uni.navigateTo({
        url: "/pages/login/login",
      });
    },
  },
};
</script>

<style scoped lang="scss">
@import "my.scss";
</style>
