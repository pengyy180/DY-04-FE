<template>
	<view>
		<view class="head-info head-info2" >
			<view class="head-logo">
				<view class="left">
					<view class="listicon"  @click="navshow = true">
					
					</view>
					<view class="logoicon" @click="onSkip('home')" >
						
					</view>
					<u-popup 
						:show="navshow" 
						@close="navclose" 
						mode="left" 
						:closeable="true"
					>
						<view class="navmodal">
							<view class="myuser">
								您好,{{userinfo.customName}} <br/>
								<text>我的賬戶</text>
							</view>
							<view class="list">
								<view class="li liblod" @click="onSkip('my')">
								<image src="../static/n1.png" mode=""></image>	我的賬戶</view>	
								<view class="li" @click="onSkip('home')">
									<image src="../static/n2.png" mode=""></image>
									Yahoo Home</view>
								<view class="li" @click="onSkip('orderlist')">
									<image src="../static/n3.png" mode=""></image>
									我的訂單</view>	
								<view class="li" @click="onSkip('mywallet')">
									<image src="../static/n4.png" mode=""></image>
									我的錢包</view>	
								<view class="li">
									<image src="../static/n5.png" mode=""></image>
									我的會員 
									<text v-if="userinfo.customLevel == 0"> 普通會員</text>
									<text v-if="userinfo.customLevel == 1"> 黃金會員</text>
									<text v-if="userinfo.customLevel == 2"> 鉑金會員</text>
								</view>	
								<view class="li" @click="onSkip('kefu')">
									<image src="../static/n6.png" mode=""></image>
									專屬客服</view>	
								<view class="li downapp" @click="downladapp">
									<image src="../static/n7.png" mode=""></image>
									下載APP</view>
								<view class="li" @click="onSkip('loginout')">
									<image src="../static/n8.png" mode=""></image>
									退出賬號</view>	

							</view>
							<!-- <text>出淤泥而不染，濯清涟而不妖</text> -->
						</view>
					</u-popup>
					
				</view>
				<view class="loginicon">
					<view class="icon-info" @click="search = true">
					   <text class="cuIcon-search"></text>
					</view>
					<view class="icon-info" v-if="logininfo.id" @click="onSkip('my')">
					   <text class="cuIcon-people"></text>
					</view>
					<view class="icon-info" v-if="!logininfo.id" @click="onSkip('login')">
					  登录 <text class="cuIcon-right"></text> <text class="cuIcon-people"></text>
					</view>
				</view>
			</view>
		  <!-- 搜索 -->
		  <view class="head-search" v-if="search == true">
			  
		    <view class="search" @click="onSearch">
		      
		      <view class="hint">
		        <text class="max">搜索 Yahoo 奇摩超级商城</text>
		        <!-- <text class="min">更多海外商品等您来</text> -->
		      </view>
			  <view class="icon"> 
				<i class="nav-sprite"></i>
			  </view>
		    </view>
		     
		    
		  </view>
		<!--  <view class="classify-list" >
		    <view
		      class="list"
		      v-for="(item, index) in classList"
		      :class="{ action: classifyShow == index }"
		      @click="onClassify(item, index)"
		      :key="index"
		    >
		      <text>{{ item.name }}</text>
		      <text class="line" v-show="classifyShow == index"></text>
		    </view>
		  </view> -->
		  
		 
		  </view>
	</view>
</template>

<script>
	import { findCustomData,findProductCat,findKefu } from "../api/index";
	export default {
		name:"navheader",
		props:{
			navShow: {
				type: Boolean,
				default: false,
			}
		},
		data() {
			return {
				search:false,
				classifyShow: 0,
				classList:[],
				classList2:[],
				navshow:false,
				userinfo:'',
				logininfo:''
			};
		},
		mounted(){

			this.checkuserinfo()
		},
		onLoad() {
			
			
		},
		onshow(){

		},
			
		methods:{
			downladapp(){
				uni.showToast({
					title: 'APP升级维护，敬请期待',
					icon: 'none'
				})

			},
			navclose(){
				this.navshow = false
			},
			checkuserinfo(){
				let userinfo = uni.getStorageSync('user')
				
				if(userinfo){
					this.logininfo = userinfo
					// this.checkuseinfo()
				}
				this.checkprotypelist()
				console.log(userinfo)
			},
			async checkprotypelist(){
					  let _this = this
					  //商品分类
					  findProductCat().then((res)=>{
						  let data = [
							  {
							    id: 0,
							    name: "首页",
							  }
						  ]
						  let data2 = []
						   res.result.forEach(function (item, index) {
							   item.coverImg = item.coverImg.split(",")
							   let li = {}
							   li.id = item.id
								li.name = item.productCatName
								li.coverImg = _this.imgaction+'/'+item.coverImg[0]
								data.push(li)
								data2.push(li)
						   });
						  this.classList = data
						  this.classList2 = data2
					  })
					  
			},
			/**
			 * 跳转点击
			 * @param {String} type 跳转类型
			 */
			onSkip(type) {
			  switch (type) {
			  case "my":
			  		uni.navigateTo({
			  		  url: "/pages/my/my",
			  		});
			  		break;
			    case "home":
			      uni.navigateTo({
			        url: "/pages/home/home",
			      });
			      break;
				  case "login":
				    uni.navigateTo({
				      url: "/pages/login/login",
				    });
				    break;
					case "orderlist":
					  uni.navigateTo({
					    url: "/pages/MyOrderList/MyOrderList",
					  });
					  break;
					 case "mywallet":
					   uni.navigateTo({
					     url: "/pages/MyWallet/MyWallet",
					   });
					   break;
					   case "kefu":
					    findKefu().then((res)=>{
					    	console.log(res.result)
					    	window.location.href="https://chatlink.mstatik.com/widget/standalone.html?eid="+res.result
					    })
					     break;
						 case "loginout":
						  
						  	uni.clearStorage();
						  	uni.reLaunch({
						  		url:'/pages/login/login'
						  	})
						  	
						  
						   break;
					  
				}
			},
			/**
			 * 搜索点击
			 */
			onSearch() {
			  uni.navigateTo({ url: "/pages/search/search" });
			},
			checkuseinfo(){
				
				let _this = this
				findCustomData().then(res=>{
					_this.userinfo = res.result
					
					if(!_this.userinfo.payPwd){
						_this.showmoadl('为了用户的安全，请设置支付密码')
						_this.settimeurl('/pages/PaymentPassword/PaymentPassword')
					}
					
				})
			},
			showmoadl(msg){
				uni.showToast({
					title: msg,
					icon:"none",
					duration: 2000
				});
			},
			settimeurl(url){
				setTimeout(()=>{
					uni.navigateTo({
						url:url
					})
				},2000)
			}
		}
		
	}
</script>

<style lang="scss">
@import "@/pages/home/home.scss";
.downapp{
	margin-bottom: 50rpx;
}
</style>