<template>
  <view class="page">
    <navheader :navShow="true"></navheader>
	<view class="myauthor" @click="uploadimg">
		<image :src="userinfo.headPortrait" mode=""></image>
		
<view class="title">
	Welcome back to Yahoo 
</view>
		
	</view>
	<view class="mytitle2">
		我的账户
	</view>
	<view class="mylist">
		<view class="li" @click="onSetting">
			<view class="leftli">
				<image src="../../static/mingcheng.png" mode=""></image>我的昵称
			</view>
			<view class="rightli">
				{{userinfo.customName}}<text class="iconfont iconjiantou-01"></text>
			</view>
		</view>
		<view class="li" >
			<view class="leftli">
				<image src="../../static/haoma.png" mode=""></image>我的号码
			</view>
			<view class="rightli">
				{{userinfo.customPhoneNum}}<text class="iconfont iconjiantou-01"></text>
			</view>
		</view>
		
	</view>
	<view class="mytitle2">
		我的钱包
	</view>
	<view class="mylist">
		<view class="li" @click="onWallet('wallet')">
			<view class="leftli">
				<image src="../../static/yue.png" mode=""></image>我的余额
			</view>
			<view class="">
				{{userinfo.givenAccount.num}}{{userinfo.givenAccount.unit}}<text class="iconfont iconjiantou-01"></text>
			</view>
		</view>
		<view class="li" >
			<view class="leftli">
				<image src="../../static/jifen.png" mode=""></image>我的积分
			</view>
			<view class="rightli">
				0分<text class="iconfont iconjiantou-01"></text>
			</view>
		</view>
		<view class="li" @click="onSkipOrder(2)">
			<view class="leftli">
				<image src="../../static/youhuiquan.png" mode=""></image>我的优惠券
			</view>
			<view class="rightli">
				0张<text class="iconfont iconjiantou-01"></text>
			</view>
		</view>
	</view>
	<view class="mytitle2">
		我的订单
	</view>
	<view class="mylist">
		<view class="li" @click="onSkipOrder(0)">
			<view class="leftli">
				<image src="../../static/dingdan.png" mode=""></image>全部订单
			</view>
			<view class="rightli">
				<text class="iconfont iconjiantou-01"></text>
			</view>
		</view>
		<view class="li" @click="onSkipOrder(1)">
			<view class="leftli">
				<image src="../../static/daifahuo.png" mode=""></image>待发货
			</view>
			<view class="rightli">
				<text class="iconfont iconjiantou-01"></text>
			</view>
		</view>
		<view class="li" @click="onSkipOrder(2)">
			<view class="leftli">
				<image src="../../static/yiwancheng.png" mode=""></image>已完成
			</view>
			<view class="rightli">
				<text class="iconfont iconjiantou-01"></text>
			</view>
		</view>
	</view>
	<view class="mytitle2">
		账户设置
	</view>
	<view class="mylist">
		<view class="li" @click="onSetting">
			<view class="leftli">
				<image src="../../static/ziliao.png" mode=""></image>个人资料
			</view>
			<view class="rightli">
				<text class="iconfont iconjiantou-01"></text>
			</view>
		</view>
		<view class="li" @click="onWallet('SignIn')">
			<view class="leftli">
				<image src="../../static/yinhangka.png" mode=""></image>银行卡设置
			</view>
			<view class="rightli">
				<text class="iconfont iconjiantou-01"></text>
			</view>
		</view>
		<view class="li" @click="onWallet('address')">
			<view class="leftli">
				<image src="../../static/shouhuo.png" mode=""></image>收货地址
			</view>
			<view class="rightli">
				<text class="iconfont iconjiantou-01"></text>
			</view>
		</view>
		<view class="li" @click="onWallet('ChangePassword')">
			<view class="leftli">
				<image src="../../static/mima.png" mode=""></image>登录密码
			</view>
			<view class="rightli">
				<text class="iconfont iconjiantou-01"></text>
			</view>
		</view>
		<view class="li" @click="onWallet('PaymentPassword')">
			<view class="leftli">
				<image src="../../static/zhifu.png" mode=""></image>支付密码
			</view>
			<view class="rightli">
				<text class="iconfont iconjiantou-01"></text>
			</view>
		</view>
		<view class="li" @click="kefu()">
			<view class="leftli">
				<image src="../../static/rexian.png" mode=""></image>官方客服
			</view>
			<view class="rightli">
				<text class="iconfont iconjiantou-01"></text>
			</view>
		</view>
		<view class="li" @click="downloadapp()">
			<view class="leftli">
				<image src="../../static/xiazai.png" mode=""></image>下载APP
			</view>
			<view class="rightli">
				<text class="iconfont iconjiantou-01"></text>
			</view>
		</view>
		<!-- <view class="li" @click="onServer('feedback')">
			<view class="leftli">
				关于我们
			</view>
			<view class="rightli">
				<text class="iconfont iconjiantou-01"></text>
			</view>
		</view> -->
	</view>
	
	
  
    <!-- <view class="gotop" @click="goup">
    	回到顶部
    </view> -->
	
    
	<!-- 我的服务 -->
	<view class="my-service">
	  <view class="title">
	    <text>关于Yahoo平台</text>
	  </view>
	  <view class="fonttx">
	  	互联网药品信息服务资格证书(京)-非经营性-2012-0005
	  </view>
	  <view class="fonttx">
	  	增值电信业务经营许可证:合字B2-20090004
	  </view>
	  <view class="fonttx">
	  	京公网安备11010502030232号
	  </view>
	  <view class="fonttx">
	  	营业执照:91110000717883277U   京ICP备11014788号
	  </view>
	  <view class="fonttx">
	  	出版物经营许可证 使用条件 京ICP备11014788号
	  </view>
	  <view class="fonttx">	
© 1996-2022，Yahoo.com, Inc.或其附属公司
	  </view>
	</view>
    
   
	
	
    <!-- tabbar -->
	<DialogBox ref="DialogBox"></DialogBox>
    <!-- <TabBar :tabBarShow="3"></TabBar> -->
  </view>
</template>

<script>
import { updateCustomData,findCustomData,findKefu } from "../../api/index";
	import navheader from "../../components/navheader.vue";//判断用户信息
import TabBar from "../../components/TabBar/TabBar.vue";
	import __config from "../../config/env.js";
export default {
  components: {
    TabBar,
	navheader

  },
  data() {
    return {
		action:__config.basePath,

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
	  uploadimg(){
	  	let _this = this
	  	uni.chooseImage({
	  		success: (chooseImageRes) => {
	  			const tempFilePaths = chooseImageRes.tempFilePaths;
	  			 uni.uploadFile({
	  				url: __config.action, //仅为示例，非真实的接口地址
	  				filePath: tempFilePaths[0],
	  				name: 'file',
	  				formData: {
	  				  biz: "temp",
	  				},
	  				success: (uploadFileRes) => {
	  					 let data = JSON.parse(uploadFileRes.data)
	  					console.log(data);
	  					
	  					updateCustomData({headPortrait:data.message,id:_this.userinfo.id}).then((res)=>{
	  						if(res.code == 200){
	  							uni.setStorage({
	  							  key: 'userinfo',
	  							  data: res.result.psCustomMain
	  							});
	  							this.checkuserinfo()
	  						}
	  					})
	  				}
	  			});
	  	
	  			
	  		}
	  	});
	  },
	  goup(){
		  uni.pageScrollTo({
		  	scrollTop: 0,
		  	duration: 1000
		  });
	  },
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
			if(res.result.headPortrait == ''){
				_this.userinfo.headPortrait = '../../static/touxiang.png'
			}else{
				_this.userinfo.headPortrait = _this.action+'/'+res.result.headPortrait
			}

			_this.userinfo.givenAccount = _this.handleMoney(_this.userinfo.givenAccount,1)
			_this.userinfo.realAccount = _this.handleMoney(_this.userinfo.realAccount/10,2)
	  	})
	  
	  },
	  downloadapp(){

		 uni.showToast({
		 	title: 'APP升级维护，敬请期待',
		 	icon: 'none'
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
		  
		  case "PaymentPassword":
		    uni.navigateTo({
		      url: "/pages/PaymentPassword/PaymentPassword",
		    });
		    break;
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
</style>
