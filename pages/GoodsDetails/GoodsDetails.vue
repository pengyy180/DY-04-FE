<template>
  <view @click="isMore = false">
    <view class="goods-head" :style="'background:rgba(255,255,255,' + PageScrollTop / 100 + ')'">
      <!-- 返回 -->
      <view class="back" @click="onBack">
        <view class="back-one" :class="{ action: PageScrollTop > 120 }">
          <text></text>
        </view>
      </view>
      <!-- tab切换 -->
      <view class="head-tab" v-if="PageScrollTop > 120">
        <view class="tab" :class="{'action':TabShow===0}" @click="onTab(0)">
          <text>商品</text>
          <text class="line"></text>
        </view>
        <view class="tab" :class="{'action':TabShow===1}" @click="onTab(1)">
          <text>评价</text>
          <text class="line"></text>
        </view>
        <view class="tab" :class="{'action':TabShow===2}" @click="onTab(2)">
          <text>详情</text>
          <text class="line"></text>
        </view>
      </view>
      <!-- 分享更多 -->
      <view class="share-more">
        <view class="share-more-one" :class="{ action: PageScrollTop > 120 }">
         <!-- <view class="list">
            <text class="iconfont icon-share"></text>
          </view> -->
          <view class="list" @click.stop="isMore = !isMore">
            <text class="iconfont icon-diandian"></text>
          </view>
        </view>
        <view class="mroe-list" v-show="isMore">
          <navigator open-type="navigate" url="/pages/home/home" class="list">
            <view class="icon">
              <text class="iconfont icon-home"></text>
            </view>
            <view class="title">
              <text>首页</text>
            </view>
          </navigator>
          <!-- <navigator class="list">
            <view class="icon">
              <text class="iconfont icon-guanzhu"></text>
            </view>
            <view class="title">
              <text>我的关注</text>
            </view>
          </navigator>
          <navigator class="list">
            <view class="icon">
              <text class="iconfont icon-zuji"></text>
            </view>
            <view class="title">
              <text>浏览记录</text>
            </view>
          </navigator> -->
        </view> 
      </view>
    </view>
    <!-- banner，标题 -->
    <view class="banner-title">
		
		
      <!-- banner -->
      <view class="banner">
        <swiper class="screen-swiper round-dot" indicator-dots="true" circular="true" autoplay="true" interval="5000"
                duration="500">
          <swiper-item v-for="(item, index) in detailsinfo.imgs" :key="index">
            <image class="imgb" :src="item"></image>
            <!-- <video src="{{item.url}}" autoplay loop muted show-play-btn="{{false}}" controls="{{false
            }}" objectFit="cover" wx:if="{{item.type == 'video'}}"></video> -->
          </swiper-item>
        </swiper>
      </view>
	  <view class="tablist">
	  	<view class="li">
	  		<image src="../../static/img/biaoqian.png"></image> iHerb优质产品
	  	</view>
	  	<view class="li ">
	  		<image src="../../static/img/biaoqian.png"></image> 畅销热卖
	  	</view>
		<view class="li">
			<image src="../../static/img/biaoqian.png"></image> 抢先购
		</view>
	  </view>
      <!-- 价格 -->
      <view class="price-info" v-show="type==0">
        <view class="price">
          <text class="min">￥</text>
          <text class="max">{{detailsinfo.productPrice}}</text>
          <text class="money">有库存</text>
		  <view class="price-start">
		  	<u-rate :value="4.8" active-color="#EEC43B" inactive-color="#b2b2b2" :allowHalf="false"></u-rate> 
			<text>4.8</text>	
		  </view>
		  <!-- <text class="del">￥{{detailsinfo.productPrice * 1.5}}.00</text> -->
		  <!-- <text class="money">返利:￥{{detailsinfo.propAccount }}.00</text> -->
		  
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
			<!-- 限时抢购 -->
			<view class="flash-price" v-show="type==1">
				<view class="price-item">
					<view class="icon-item">
						<text class="iconfont icon-flash-sale"></text>
					</view>
					<view class="price">
						<view class="current-price">
							<text class="min">￥</text>
							<text class="max">{{detailsinfo.productPrice}}</text>

						</view>
						<view class="original-price">
							<text>￥149.00</text>
						</view>
					</view>
					<view class="tag">
						<text class="iconfont icon-flash-naozhong"></text>
					</view>
				</view>
				<view class="time-item">
					<view class="title">
						<text>距结束还剩：</text>
					</view>
					<view class="time">
						<text class="num">02</text>
						<text class="dot">:</text>
						<text class="num">46</text>
						<text class="dot">:</text>
						<text class="num">52</text>
					</view>
				</view>
			</view>
      <!-- 标题 -->
      <view class="goods-title">
        <text>{{detailsinfo.productTitle}}</text>
      </view>
      <!-- 开通会员 -->
      <view class="dredge-vip">
        <view class="title">
          <text class="iconfont icon-vip"></text>
          <text>
            开通年卡会员预计赠送
            <text class="col">￥100 </text>
            优惠券
          </text>
        </view>
        <view class="dredge">
          <text>立即</text>
          <text>开通</text>
        </view>
      </view>
    </view>
    <!-- 优惠积分 -->
    <view class="goods-discounts">
		<view class="list">
		  <view class="title">上架</view>
		  <view class="content">
		    <text>{{detailsinfo.createTime}}</text>
		  </view>
		  <view class="more">
		    <text class="iconfont icon-more"></text>
		  </view>
		</view>
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
          <text>已选	{{buynumber}}件</text>
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
    <!-- <view class="evaluate-data" ref="evaluate">
      <view class="title-more" @click="onEvaluate">
        <view class="title">
          <text>评价</text>
          <text class="num">999+</text>
        </view>
        <view class="more">
          <text class="iconfont icon-more"></text>
        </view>
      </view>
      <view class="evaluate-list">
        <view class="user-info">
          <view class="thumb">
            <image src="/static/img/user_pic.jpg" mode=""></image>
          </view>
          <view class="nickname-grade">
            <view class="nickname">
              <text>爱笑的汤姆</text>
            </view>
            <view class="grade">
              <text class="cuIcon-favorfill lg text-gray"></text>
            </view>
          </view>
        </view>
        <view class="content">
          <view class="character">
            <text class="two-omit">搭建啊激动了阿建档立卡点击就阿卡丽登记卡加端口几啊开了都金坷垃就恐龙当家哦架空</text>
          </view>
          <view class="attr">
            <text>蓝色</text>
          </view>
          <view class="thumb-list">
            <view class="list">
              <image src="/static/img/goods_banner_01.webp" mode=""></image>
            </view>
            <view class="list">
              <image src="/static/img/goods_banner_02.webp" mode=""></image>
            </view>
            <view class="list">
              <image src="/static/img/goods_banner_03.webp" mode=""></image>
            </view>
          </view>
        </view>
        <view class="look-all" @click="onEvaluate">
          <text>查看全部评价</text>
        </view>
      </view>
    </view> -->
    <!-- 排行榜 -->
    <!-- <view class="ranking-list">
      <view class="ranking-title">
        <view class="title">排行榜</view>
      </view>
      <view class="goods-list">
        <view class="list" v-for="(item, index) in 6" :key="index">
          <view class="thumb">
            <image :src="'/static/img/goods_thumb_0'+(index+1)+'.png'"></image>
          </view>
          <view class="title">
            <text class="two-omit">美连诚雪纺连衣裙 2020新款女夏裙子波点气质沙滩裙仙气时尚女装休闲衣服大码女装 白底红点 M</text>
          </view>
          <view class="price">
            <text>￥121.00</text>
          </view>
        </view>
      </view>
    </view> -->
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
        <!-- <view class="cart-add" @click="$refs['GoodsAttr'].show(2)">
          <text>加入购物车</text>
        </view> -->
<!--       <view class="buy-at" @click="$refs['GoodsAttr'].show(3)">
         <text>立即购买</text>
       </view> -->
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
					<image :src="detailsinfo.imgs[0]" mode=""></image>
				</view>
				<view class="item">
					<view class="title">
						<text></text>
					</view>
					<view class="price">
						<text class="min">￥</text>
						<text class="max">{{detailsinfo.productPrice}}</text>

					</view>
					<view class="inventory">
						<text>库存：3000</text>
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
					
							<input type="tel" @click.stop="pushmoney" v-model="buynumber"  placeholder="请输入数量" style="text-align: center;">
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
	import CodeKeyboard from '../../components/CodeKeyboard/CodeKeyboard.vue';
export default {
  components: {
	  CodeKeyboard,
    GoodsServe,
    GoodsCoupon,
    GoodsAttr,
	checkdata
  },
  data() {
    return {
		selectPort:0,
		moneylist:[
			{money:1},
			{money:20},
			{money:50},
			{money:100},
			{money:200},
			{money:500},
		],
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
			userinfo:[]
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
					// }
		if(value){
			this.checkuserinfo()
		}
		this.type = params.type||0;
	},
	onPageScroll(e) {
		this.PageScrollTop = e.scrollTop;
	},
	
  methods: {
	  changePort(index) {
	  			
	  			this.selectPort = index;
	  			this.buynumber = this.moneylist[index].money
	  			
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
				console.log(value)
			// }
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
			  				url:'/pages/OrderDetails/OrderDetails?id='+res.result
			  			})
						uni.reLaunch({
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
