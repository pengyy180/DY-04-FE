<template>
  <view class="page">
	  <!-- 用户信息 -->
    <!-- <view class="my-top">

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
        
        </view>
      </view>
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
      
    </view> -->
	<homenav></homenav>
	
	<view class="procat">
		<view class="protitle">
			我的账户
		</view>
	</view>
	<view class="border-radius" >
		<view class="vip-card-area pos-r padding-lr padding-tb-sm" @click="kefu">
			<view class="text-just">
				<view >
					<text class="iconfont iconhuiyuan"></text>
					<text class="margin-left-sm" style="margin-right: 10rpx;">Naturecan商城 </text>
					<text v-if="userinfo.customLevel == 0"> 普通会员</text>
					<text v-if="userinfo.customLevel == 1"> 黄金会员</text>
					<text v-if="userinfo.customLevel == 2"> 铂金会员</text>
				</view>	
				<text class="margin-left-sm border-r">立即申请</text>
			</view>
		</view>
		<view class="order-info">
			
				<view class="list-f">
				  <view class="list" @click="onSkipOrder(0)">
					<view class="icon">
					  <text class="iconfont icon-daifukuan"></text>
					</view>
					<view class="title">
					  <text>全部</text>
					</view>
				  </view>
				  <view class="list" @click="onSkipOrder(1)">
					<view class="icon">
					  <text class="iconfont icon-daifahuo"></text>
					</view>
					<view class="title">
					  <text>待发货</text>
					</view>
				  </view>
				  <view class="list" @click="onSkipOrder(2)">
					<view class="icon">
					  <text class="iconfont icon-daishouhuo"></text>
					</view>
					<view class="title">
					  <text>已完成</text>
					</view>
				  </view>
		  </view>
		</view>
		<!-- <view class="moneylist">
			<view class="li" >
				<view class="number">{{userinfo.givenAccount.num}}{{userinfo.givenAccount.unit}}</view>
				<view class="title">我的余额</view>
			</view>
			<view class="li">
				<view class="number">0</view>
				<view class="title">我的积分</view>
			</view>
			<view class="li">
				<view class="number">0</view>
				<view class="title">优惠券</view>
			</view>
		</view> -->
		
	</view>
	
    <view class="border-radius" >
    	<view class="vip-card-area pos-r padding-lr padding-tb-sm" @click="kefu">
    		<view class="text-just">
    			<view >
    				<text class="iconfont icon-qianbao"></text>
    				<text class="margin-left-sm" style="margin-right: 10rpx;">我的钱包 </text>
    			</view>	
    		</view>
    	</view>
    	
    	<view class="moneylist">
    		<view class="li" >
    			<view class="number">{{userinfo.givenAccount.num}}{{userinfo.givenAccount.unit}}</view>
    			<view class="title">我的余额</view>
    		</view>
    		<view class="li">
    			<view class="number">0</view>
    			<view class="title">我的积分</view>
    		</view>
    		<view class="li">
    			<view class="number">0</view>
    			<view class="title">优惠券</view>
    		</view>
    	</view>
    	
    </view>
    <view class="border-radius border-radius2" >
    	<view class="vip-card-area pos-r padding-lr padding-tb-sm" @click="kefu">
    		<view class="text-just">
    			<view >
    				<text class="cuIcon-list"></text>
    				<text class="margin-left-sm" style="margin-right: 10rpx;">更多功能 </text>
    			</view>	
    		</view>
    	</view>
		<view class="borderff">
			<view class="title-v" @click="onSetting">
				<view class="left">
					<image class="iconimg" src="../../static/my1.png" mode=""></image>个人资料
				</view>
				<view class="right">
					填写信息 <text class="iconfont iconjiantou-01"></text>
				</view>
			</view>	
			<view class="title-v" @click="onWallet('SignIn')">
				<view class="left">
					<image class="iconimg" src="../../static/my2.png" mode=""></image>银行卡绑定
				</view>
				<view class="right">
					银行卡设置 <text class="iconfont iconjiantou-01"></text>
				</view>
			</view>	
			<view class="title-v" @click="onWallet('wallet')">
				<view class="left">
					<image class="iconimg" src="../../static/my3.png" mode=""></image>我的钱包
				</view>
				<view class="right">
					钱包管理 <text class="iconfont iconjiantou-01"></text>
				</view>
			</view>	
			<view class="title-v" @click="onWallet('address')">
				<view class="left">
					<image class="iconimg" src="../../static/my4.png" mode=""></image>收货地址
				</view>
				<view class="right">
					收货地址 <text class="iconfont iconjiantou-01"></text>
				</view>
			</view>	
			<view class="title-v" @click="onWallet('ChangePassword')">
				<view class="left">
					<image class="iconimg" src="../../static/my5.png" mode=""></image>密码设置
				</view>
				<view class="right">
					密码设置 <text class="iconfont iconjiantou-01"></text>
				</view>
			</view>	
			<view class="title-v" @click="kefu()">
				<view class="left">
					<image class="iconimg" src="../../static/my6.png" mode=""></image>官方客服
				</view>
				<view class="right">
					在线客服 <text class="iconfont iconjiantou-01"></text>
				</view>
			</view>	
			<view class="title-v" @click="onServer('feedback')">
				<view class="left">
					<image class="iconimg" src="../../static/my7.png" mode=""></image>关于Naturecan
				</view>
				<view class="right">
					了解更多 <text class="iconfont iconjiantou-01"></text>
				</view>
			</view>	
		</view>
    </view>
    <!-- 钱包 -->
	
	
    <!-- <view class="wallet-info">
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

        <view class="icon">
          <text class="number">{{userinfo.givenAccount}}</text>
        </view>
        <view class="title">
          <text>余额</text>
        </view>
      </view>
    </view>
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
    </view> -->
    <!-- 我的服务 -->
    <!-- <view class="my-service">
      <view class="title">
        <text>我的服务</text>
      </view>
      <view class="service-list">

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

            <text class="cuIcon-people" style="font-size: 20px"></text>
          </view>
          <view class="name">
            <text>客服热线</text>
          </view>
        </view>
        <view class="list" >
          <view class="thumb">
            <text class="cuIcon-exit" style="font-size: 20px"></text>
			</view>
          <view class="name" @click="loginout">
            <text>退出登录</text>
          </view>
        </view>
      </view>
    </view> -->
	<!-- 我的服务 -->
	
	<view class="aboutme">
		<view class="li">
			<image class="aboutimg1" src="/static/a1.webp" mode=""></image>
		</view>
		<view class="li">
			<image class="aboutimg1" src="/static/a2.webp" mode=""></image>
		</view>
		<view class="li">
			<image class="aboutimg2" src="/static/a3.webp" mode=""></image>
			<image class="aboutimg2" src="/static/a4.webp" mode=""></image>
		</view>
		<view class="li">
			<image class="aboutimg3" src="/static/a5.webp" mode=""></image>
			<image class="aboutimg4" src="/static/a6.webp" mode=""></image>
		</view>
	</view>
	<view class="copyright">
		© 2022, Naturecan Ltd - Bank Chambers, Stockport, SK1 1AR, UK
	</view>
	<view class="china">
		🇨🇳 CHINA
	</view>
	<view class="smallabout">
		This product is not for use by or sale to persons under the age of 18. This product should be used only as directed on the label. It should not be used if you are pregnant or nursing. Consult with a physician before use if you have a serious medical condition or use prescription medications. A Doctor's advice should be sought before using this and any supplemental dietary product. All trademarks and copyrights are property of their respective owners and are not affiliated with nor do they endorse this product. These statements have not been evaluated by the FDA. This product is not intended to diagnose, treat, cure or prevent any disease. Individual weight loss results will vary. By using this site, you agree to follow the Privacy Policy and all Terms & Conditions printed on this site. Void Where Prohibited by Law.
	</view>
	<!-- <view class="my-service">
	  <view class="title">
	    <text>关于平台</text>
	  </view>
	  <view class="service-list" style="color: #959595;text-align: center;padding-bottom: 20rpx;">
	    Naturecan商城于1996年美国成立，2010年来到中国，向全世界150多个国家提供商品。多年来，Naturecan获得了令人曙目的成绩。

	  </view>
	</view> -->
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
	import homenav from "../../components/homenav.vue";//判断用户信息
export default {
  components: {
    TabBar,
	homenav

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
	  handleMoney(num,ok) {
	    // 首先先声明一个金额单位数组
		let AmountUnitlist = ["元", "万元", "亿", "兆", '京', '垓', '杼']
		if(ok == 1){
			AmountUnitlist = ["元", "万元", "亿", "兆", '京', '垓', '杼']
		}else if(ok == 2){
			AmountUnitlist = ["分", "万分", "亿", "兆", '京', '垓', '杼']
		}
	    // 将数字金额转为字符串
	    let strnum = num.toString()
	    // 声明一个变量用于接收金额单位
	    let AmountUnit = ''
	    // 循环遍历单位数组
	    AmountUnitlist.find((item, index) => {
	      let newNum = ''
	      // 判断一下传进来的金额是否包含小数点
	      if (strnum.indexOf('.') !== -1) {
	        // 若有则将小数点前的字符截取出来
	        newNum = strnum.substring(0, strnum.indexOf('.'))
	      } else {
	        // 没有则直接等于原金额
	        newNum = strnum
	      }
	      // 判断一下经过小数点截取后的金额字符长度是否小于5
	      if (newNum.length < 5) {
	        // 若小于5则接收当前单位，并跳出迭代
	        AmountUnit = item
	        return true
	      } else {
	        // 若不小于5则将经过小数点截取处理过后的字符除以10000后作为下一轮迭代的初始金额重新判断(每一个单位之间相距4位数，故除以10000)
	        strnum = (newNum * 1 / 10000).toString()
	      }
	    })
	    let money = {num: 0, unit: ""}
	    // 保留2位小数
	    money.num = (strnum * 1).toFixed(2)
	    // 接收单位
	    money.unit = AmountUnit
	    return money
	  },
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
			_this.userinfo.givenAccount = _this.handleMoney(_this.userinfo.givenAccount,1)
			_this.userinfo.realAccount = _this.handleMoney(_this.userinfo.realAccount/10,2)
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
		  
		  case "ChangePassword":
		    uni.navigateTo({
		      url: "/pages/ChangePassword/ChangePassword",
		    });
		    break;
		case "address":
		  uni.navigateTo({
		    url: "/pages/AddressEdit/AddressEdit",
		  });
		  break;
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
            // url: "/pages/ConsumeRecord/ConsumeRecord?type=0",
			url: "/pages/MyWallet/MyWallet",
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
@import "@/pages/home/home.scss";
</style>
