<template>
  <view class="page">
    <view class="goods-head" >
		<navheader :navShow="true"></navheader>
      <!-- 返回 -->
      
      <!-- 分享更多 -->
     
    </view>
    <!-- banner，标题 -->
    <view class="banner-title">
		
		<view class="protitle">
			<view class="title">
				{{detailsinfo.productTitle}}
			</view>
			<view class="start">
				<u-rate :value="4.8" active-color="#ffa41c" inactive-color="#b2b2b2" :allowHalf="false"></u-rate> 4.8
			</view>
		</view>
		<view class="protxtimg">
			<view class="sq1">
				海外自营
			</view>
			<view class="sq2">
				人气
			</view>
			<view class="sq3">
				<image src="../../static/d1.svg" mode=""></image>
			</view>
			<view class="sq3">
				<image src="../../static/d2.svg" mode=""></image>
			</view>
			<view class="sq3">
				<image src="../../static/d3.svg" mode=""></image>
			</view>
		</view>
      <!-- banner -->
      <view class="banner">
        <swiper class="screen-swiper round-dot" indicator-dots="true" circular="true" autoplay="true" interval="5000"
                duration="500">
          <swiper-item v-for="(item, index) in detailsinfo.imgs" :key="index">
            <image class="imgb" :src="item" mode="aspectFit"></image>
           
          </swiper-item>
        </swiper>
      </view>
	  <!-- 水平线 -->
	 <view class="hrccc"></view>
	 
	 <view class="pronum">
	 	<view class="title">
	 		数量
	 	</view>
		<view class="list" >
			<view class="li" v-for="(item,index) in moneylist" :class="{action:index==selectPort}"
					:key="index"
					@click.stop="changePort(index)">
				<view class="number">
					{{item.num}}件
				</view>
				<view class="price">
					￥{{item.productPrice}}
				</view>
				<view class="green">
					现在有货
				</view>
			</view>
		</view>
	 </view>
      <!-- 价格 -->
      <view class="price-info" >
        <view class="price">
          <text class="min">￥</text>
          <text class="max">{{buynumber}}</text>
        </view>
		
        <view class="info">
         <!-- <view class="list" @click="onDepreciate">
            <text class="iconfont icon-jiangjia"></text>
            <text>降价通知</text>
          </view> -->
          <view class="list" @click="onAttention">
            <text class="iconfont" :class="AttentionShow===0?'icon-guanzhu-off':'icon-guanzhu-on action'"></text>
            <text>{{ AttentionShow === 0 ? '关注' : '已关注' }}</text>
          </view>
        </view>
      </view>

		  <view class="jsj">
			 <text class="del">进口税+¥{{buynumber * 0.06}}</text> 限时活动减税
		  </view>
		  <view class="primetext">
		  	<text>✓</text> prime
		  </view>
		  <view class="hwg">
				<image src="../../static/hwg.png" mode=""></image>
		  </view>

			
      <!-- 标题 -->
     
      
    </view>
    <!-- 优惠积分 -->
    <view class="goods-discounts">
      <view class="list">
        <view class="title">积分</view>
        <view class="content">
          <text>购买本商品可获得100积分</text>
        </view>
        <view class="more">
          <text class="iconfont icon-more"></text>
        </view>
      </view>
      <view class="list" @click="$refs['GoodsServe'].show()">
        <view class="title">服务</view>
        <view class="content">
          <view class="serve">
            <text class="iconfont icon-baozheng"></text>
            <text>退款保证</text>
          </view>
          <view class="serve">
            <text class="iconfont icon-baozheng"></text>
            <text>物流配送</text>
          </view>
        </view>
        <view class="more">
          <text class="iconfont icon-more"></text>
        </view>
      </view>
      <view class="list" @click="$refs['GoodsCoupon'].show()">
        <view class="title">领券</view>
        <view class="content">
          <view class="coupon-list">
            <view>满50减10</view>
          </view>
          <view class="coupon-list">
            <view>满19减5</view>
          </view>
        </view>
        <view class="more">
          <text class="iconfont icon-more"></text>
        </view>
      </view>
    </view>
    <!-- 属性规格 -->
    <view class="goods-discounts" >
      <view class="list" @click="isShow=true">
        <view class="title">已选</view>
        <view class="content">
          <text>已选	{{buynum}}件</text>
        </view>
        <view class="more">
          <text class="iconfont icon-more"></text>
        </view>
      </view>
      <view class="list" @click="gourl">
        <view class="title">送至</view>
        <view class="content">
          <view class="serve">
            <text class="iconfont icon-dingwei"></text>
            <text >{{userinfo.addressProvi == null ? '请录入地址':userinfo.addressProvi}}</text>
          </view>
        </view>
        <view class="more">
          <text class="iconfont icon-more"></text>
        </view>
      </view>
      <view class="list">
        <view class="title">运费</view>
        <view class="content">
          <text>免运费</text>
        </view>
        <view class="more"><!-- <text class="iconfont icon-more"></text> --></view>
      </view>
    </view>
    <!-- 评价 -->
   
    <!-- 商品介绍 -->
    <view class="products-introduction" ref="products">
      <view class="title">
        <text>商品介绍</text>
      </view>
      <view class="content" v-html="detailsinfo.productDescString"></view>
    </view>
    <!-- 底部 -->
    <view class="page-footer">
      <!-- <view class="footer-fn">
        <view class="list">
          <text class="iconfont icon-kefu"></text>
          <text>联系客服</text>
        </view>
        <view class="list" @click="onToCart">
          <text class="iconfont icon-cart"></text>
          <text>购物车</text>
        </view>
      </view> -->
      <view class="footer-buy">
       
	   <view class="buy-at" @click="checkuserlogin">
          <text>立即购买</text>
        </view>
		
		<uni-popup ref="popup" :is-mask-click="false" >
			<view class="bgmodal">
				<view class="title">
					请输入支付密码
				</view>
				<view class="pay-password" >
					<view class="list">
						<text v-show="passwordArr.length >= 1">●</text>
					</view>
					<view class="list">
						<text v-show="passwordArr.length >= 2">●</text>
					</view>
					<view class="list">
						<text v-show="passwordArr.length >= 3">●</text>
					</view>
					<view class="list">
						<text v-show="passwordArr.length >= 4">●</text>
					</view>
					<view class="list">
						<text v-show="passwordArr.length >= 5">●</text>
					</view>
					<view class="list">
						<text v-show="passwordArr.length >= 6">●</text>
					</view>
				</view>
				
			</view>
		</uni-popup>
		<CodeKeyboard ref="CodeKeyboard" passwrdType="pay" @KeyInfo="KeyInfo"></CodeKeyboard>
      </view>
    </view>
    <!-- 服务弹窗 -->
    <goods-serve ref="GoodsServe"></goods-serve>
    <!-- 优惠券 -->
    <goods-coupon ref="GoodsCoupon"></goods-coupon>
    <!-- 属性规格 -->
    <goods-attr ref="GoodsAttr" :datainfo="detailsinfo" @buy = "onPayUp"></goods-attr>
	
	<!-- 数量 -->
	<view class="cu-modal bottom-modal" :class="{'show':isShow}" @click.stop="hide">
	  <view class="cu-dialog">
			<view class="goods-data">
				<view class="thumb">
					<image :src="detailsinfo.imgs[0]" mode="aspectFit"></image>
				</view>
				<view class="item">
					<view class="title">
						<text></text>
					</view>
					<view class="price">
						<text class="min">￥</text>
						<text class="max">{{buynumber}}</text>

					</view>
					<view class="inventory">
						<text>库存：9999</text>
					</view>
				</view>
			</view>
			<view class="attr-size">
				<view class="titile" style="width: 100%;text-align: left;height: 30px;margin-top: 20px;">快捷数量</view>
				<view class="recharge-list">
					<!-- <view class="list" v-for="(item,index) in moneylist" 
					:class="{action:index==selectPort}"
					:key="index"
					@click.stop="changePort(index)">
						<view class="price">
							<text>{{item.money}}个</text>
						</view>
					</view> -->
					<view class="list" >
						<view class="price">
					
							<input type="tel" @input="moneyup" @click.stop="pushmoney" v-model="buynum"  placeholder="请输入数量" style="text-align: center;">
						</view>
					</view>
				</view>
				<!-- <view class="attr-number" >
					<view class="tit">数量</view>
					<view class="number">
						<text class="iconfont icon-jian" @click.stop="jiannumber"></text>
						<input type="tel" v-model="buynumber" value="1" maxlength="8">
						<text class="iconfont icon-jia" @click.stop="jianumber"></text>
					</view>
					
				</view> -->
			</view>
			<view class="attr-btn">
				<view class="add-buy" @click="onPayUp">立即购买</view>
			</view>
			
			<!-- <view class="buy-at" @click="onPayUp">
			   <text>立即购买</text>
			 </view> -->
		</view>
	</view>
	<!-- <checkdata></checkdata> -->
	<!-- 数量 -->
  </view>
</template>

<script>
import GoodsServe from '../../components/GoodsServe/GoodsServe.vue';
import GoodsCoupon from '../../components/GoodsCoupon/GoodsCoupon.vue';
import GoodsAttr from '../../components/GoodsAttr/GoodsAttr.vue';
import { findProductDetail } from "../../api/index";
	import { updateCustomData,findCustomData ,buyOrder} from "../../api/index";
import __config from "../../config/env.js";
	import checkdata from "../../components/checkdata.vue";//判断用户信息
	import navheader from "../../components/navheader.vue";//判断用户信息
	import CodeKeyboard from '../../components/CodeKeyboard/CodeKeyboard.vue';
export default {
  components: {
	  CodeKeyboard,
    GoodsServe,
    GoodsCoupon,
    GoodsAttr,
	checkdata,
	navheader
  },
  data() {
    return {
		selectPort:0,
		moneylist:[],
		buynumber:1,
		isShow: false,
		action: __config.basePath, // 图片显示地址
      TabShow: 0,
      isMore: false,
      AttentionShow: 0,
      swiperList: [
        {
          id: 0,
          type: 'image',
          url: '/static/img/goods_banner_01.webp'
        },
        {
          id: 1,
          type: 'image',
          url: '/static/img/goods_banner_02.webp'
        },
        {
          id: 2,
          type: 'image',
          url: '/static/img/goods_banner_03.webp'
        },
        {
          id: 3,
          type: 'image',
          url: '/static/img/goods_banner_04.webp'
        },
        {
          id: 4,
          type: 'image',
          url: '/static/img/goods_banner_05.webp'
        },
      ],
      web_content:
          '<div class="m-img"><img src="https://zhedplus.oss-cn-hangzhou.aliyuncs.com/content_img/20191118/1fb5ff162f25fd4c7383bd998ff2fde9.jpg"><div class="tools" hidden><i class="fa fa-arrow-up move-up"></i><i class="fa fa-arrow-down move-down"></i><em class="move-remove" hidden ><i class="fa fa-times" aria-hidden="true"></i> 移除</em><div class="cover"></div></div></div>',
      PageScrollTop: 0,
			type: 0,
			
			detailsinfo:'',
			AffirmStatus: 1,
			passwordArr: [],
			oldPasswordArr: [],
			newPasswordArr: [],
			afPasswordArr: [],
			userinfo:[],
			buynum:1
    };
  },
	onLoad(params) {
		console.log(params)
		
		if(params.id){
			this.checkdetails(params.id)
		}else{
			
		}
		
		let value = uni.getStorageSync('token')
						console.log(value)
					
		if(value){
			this.checkuserinfo()
		}
		this.type = params.type||0;
	},
	onPageScroll(e) {
		this.PageScrollTop = e.scrollTop;
	},
	
  methods: {
	  moneyup(){
		  if(this.buynum < 0){
			  uni.showToast({
			  	title: '请输入1以上的数量',
			  	icon:"none",
			  	duration: 1000
			  });
			 
			  return
		  }
		  console.log('11111111111')
		  this.buynumber = this.buynum * this.moneylist[0].productPrice
	  },
	  changePort(index) {
	  			
	  			this.selectPort = index;
	  			this.buynumber = this.moneylist[index].productPrice
				this.buynum = this.moneylist[index].num
	  			
	  	},
	  checkuserinfo(){
	  	let _this = this
	  	
	  	findCustomData().then(res=>{
	  		console.log(res)
	  		_this.userinfo = res.result
			
			if(!res.result.payPwd){
				
				uni.showToast({
					title: '为了用户的安全，请设置支付密码',
					icon:"none",
					duration: 1000
				});
				setTimeout(()=>{
					uni.navigateTo({
						url:'/pages/PaymentPassword/PaymentPassword'
					})
				},1000)
			}else if(!res.result.addressName){
				
				uni.showToast({
					title: '为了更好的体验，请填写用户收货地址',
					icon:"none",
					duration: 1000
				});
				setTimeout(()=>{
					uni.navigateTo({
						url:'/pages/AddressEdit/AddressEdit'
					})
				},1000)
			}
			
	  	})
	  
	  },
	  /**
	   * 唤起键盘
	   */
	  checkuserlogin(){
		  let value = uni.getStorageSync('token')
		  				console.log(value)
		  			// }
		  if(value){
		  			  this.isShow = true
		  			    
		  			  //   this.$refs.popup.open('center')
		  			  // this.$refs['CodeKeyboard'].show();
		  }else{
		  			  console.log('111111111111111111')
					  uni.showToast({
					  	title: '请登录进行购买',
						icon:'error',
					  	duration:2000
					  })

					  setTimeout(function () {
					    uni.reLaunch({
					      url: "/pages/login/login"
					    })
					  }, 1000)
		  }
	  },
	  onPayUp(){
			// if(uni.getStorageSync('token')){
				
				let value = uni.getStorageSync('token')
			
			
			if(this.buynum == 0){
				uni.showToast({
					title: '请输入数量',
					icon:'none',
					duration:1000
				})
				this.buynum  = 1
				this.buynumber  = this.moneylist[0].productPrice
				return
			}
		  if(value){
			  
			    
			  //   this.$refs.popup.open('center')
			  // this.$refs['CodeKeyboard'].show();
			  let data = {
			  	productNum:this.buynumber,
			  	productId:this.detailsinfo.id
			  }
			  buyOrder(data).then((res)=>{
			  	
			  	if(res.code == 200){
			  		console.log(res)
			  		uni.showToast({
			  			title: '支付成功',
			  			duration:2000
			  		})
			  		setTimeout(function () {
			  			uni.navigateTo({
			  				// url:'/pages/OrderDetails/OrderDetails?id='+res.result
			  				url:'/pages/home/home'
			  			})
			  		}, 2000);
			  	}
			  })
		  }else{

			  this.checkuserlogin()
		  }
	  },
	  /**
	   * 支付键盘回调
	   * @param {Object} val
	   */
	  KeyInfo(val){
		  console.log(val)
	  	if(val.index >= 6){
	  		return;
	  	}
	  	// 判断是否删除
	  	if(val.keyCode === 8){
	  		this.passwordArr.splice(val.index+1,1)
	  	}else{
	  		this.passwordArr.push(val.key);
	  	}
	  	// 判断是否等于6
	  	if(this.passwordArr.length === 6){
	  		this.passwordArr = [];
	  		this.AffirmStatus = this.AffirmStatus+1;
	  	}
	  	// 判断到哪一步了

	  	if(this.AffirmStatus === 1){
	  		this.oldPasswordArr = this.passwordArr;
	  	}else{
	  		if(this.oldPasswordArr.join('') == this.userinfo.payPwd){
	  			
	  			let pwd = this.afPasswordArr.join('')
	  			this.$refs.popup.close()
	  			this.$refs['CodeKeyboard'].hide();

				this.oldPasswordArr = []
				this.passwordArr = []
				this.AffirmStatus = 1
				
				
				
				let data = {
					productNum:this.buynumber,
					productId:this.detailsinfo.id
				}
	  			buyOrder(data).then((res)=>{
					
	  				if(res.code == 200){
						console.log(res)
	  					uni.showToast({
	  						title: '支付成功',
							duration:2000
	  					})
						setTimeout(function () {
							uni.navigateTo({
								url:'/pages/OrderDetails/OrderDetails?id='+res.result
							})
						}, 2000);
	  				}
	  			})
	  			
	  		}else{
				this.oldPasswordArr = []
	  			uni.showToast({
	  				title: '密码不一致',
	  				icon: 'none'
	  			})
	  			this.AffirmStatus = 1

	  		}
			
	  		
	  
	  
	  
	  		
	  	}
	  	this.$forceUpdate();
	  },
	  checkdetails(id){
		  findProductDetail(id).then(res=>{
			  this.detailsinfo = res.result
			  this.buynumber = res.result.productPrice
			  
			  let number = [
				  {num:1},
				  {num:20},
				  {num:50},
				  {num:100},
				  {num:200},
				  {num:500},
			  ]
			  number.forEach(e=>{
				  e.productPrice = e.num * res.result.productPrice
			  })
			  
			  
			  this.moneylist = number
			  
			  
			  
				let imglist = res.result.productImgs.split(",")
				let imgs = []
				for (let i = 0; i < imglist.length; i++) {
					
					// let imglists = this.goodsList[i].productImgs.split(",")
					imgs.push(this.action + '/' +imglist[0])
				}
				
				this.detailsinfo.productDescString = this.detailsinfo.productDescString.replace(
				              /<img/g,
				              "<img style='min-width:50%';height:auto;"
				            )
			  this.detailsinfo.imgs = imgs
			  console.log(this.detailsinfo.imgs)
		  })
	  },
    /**
     * 返回
     */
    onBack() {
      uni.navigateBack();
    },
    /**
     * tab
     */
    onTab(type) {
      this.TabShow = type;
      switch (type) {
        case 0:
          uni.pageScrollTo({
          	scrollTop: 0,
          	duration: 300
          });
          break;
        case 1:
          uni.createSelectorQuery().select(".evaluate-data").boundingClientRect((data) => { //data - 各种参数
            uni.pageScrollTo({
							scrollTop: this.PageScrollTop + data.top -50,
							duration: 300
						});
          }).exec()
          break;
        case 2:
          uni.createSelectorQuery().select(".products-introduction").boundingClientRect((data) => { //data - 各种参数
            uni.pageScrollTo({
            	scrollTop: this.PageScrollTop + data.top - 50,
            	duration: 300
            });
          }).exec()
          break;
      }
    },
    /**
     * 去购物车
     */
    onToCart() {
      uni.navigateTo({
        url: '/pages/cart/cart'
      })
    },
    /**
     * 降价通知点击
     */
    onDepreciate() {
      uni.showToast({
        title: '降价通知提醒成功',
        icon: 'success'
      })
    },
    /**
     * 关注点击
     */
    onAttention() {
      if (this.AttentionShow === 0) {
        this.AttentionShow = 1;
        uni.showToast({
          title: '关注成功',
          icon: 'none'
        })
      } else {
        this.AttentionShow = 0;
        uni.showToast({
          title: '取消成功',
          icon: 'none'
        })
      }

    },
		/**
		 * 评价点击
		 */
		onEvaluate(){
			uni.navigateTo({
				url: '/pages/GoodsEvaluateList/GoodsEvaluateList'
			})
		},
		gourl(){
			uni.navigateTo({
				url:'/pages/AddressEdit/AddressEdit'
			})
		},
		hide(){
				this.isShow = false;
			},
			jiannumber(){
				
				if(this.buynumber>1){
					--this.buynumber
				}
			},
			jianumber(){
				++this.buynumber
			},
			pushmoney(){
				
			}
  }
};
</script>

<style scoped lang="scss">
@import 'GoodsDetails.scss';
</style>
