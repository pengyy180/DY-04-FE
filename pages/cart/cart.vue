<template>
	<view class="page">
		<!-- 头部背景 -->
		<view class="head-bg">
			<view class="head-user">
				<view class="user">
				</view>
				<view class="exchange" @click="onConsumeRecord">
					<text>提现记录</text>
					<text class="iconfont icon-more"></text>
				</view>
			</view>
			<view class="list">
				<view class="left">
					到账银行卡
				</view>
				<view class="right" @click="gosetbank">
					<view><text class="iconfont icon-qianbao" style="margin-right: 20rpx;"></text>{{userinfo.bankName==null?'请绑定银行卡':userinfo.bankName}}银行卡</view>
					<view class="ccc">5-15分钟到账</view>
				</view>
			</view>
		</view>
		<!-- 钱包充值 --> 
		<view class="wallet-recharge">
			<!-- <view class="recharge-title">
				<text>提现金额</text>
			</view> -->
			<view style="width: 100%;height: 50rpx;"></view>
			<view class="recharge-list">
				
				<view class="list" >
					<view class="price">
						<text>可提现余额:{{userinfo.givenAccount}}</text>
						<input type="text" v-model="money" placeholder="请输入提现金额" style="text-align: center;">
					</view>
				</view>
			</view>
		</view>
		
		<!-- <view class="iconlist">
			<view class="list">
				<image src="/static/img/yhk.png" mode=""></image>银行转账
			</view>
			<view class="list">
				<image src="/static/img/kf.png" mode=""></image>客服转账
			</view>
			<view class="list">
				<image src="/static/img/wx.png" mode=""></image>微信支付
			</view>
			<view class="list">
				<image src="/static/img/zfb.png" mode=""></image>支付宝支付
			</view>
		</view> -->
		<view class="recharge-btn" >
			<!-- <view class="btn" @click="tx">立即提现</view> -->
			<view class="btn" v-if="timeshow == 0" @click="tx">立即提现</view>
			<view class="btn2" v-else-if="timeshow == 1">提现时间{{starttime}}:00-{{endtime}}:00</view>
		</view>
		<!-- tabbar -->
		<DialogBox ref="DialogBox"></DialogBox>
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
		<u-keyboard 
		mode="number"
		@change="valChange" 
		@backspace="backspace" 
		:dotDisabled="true" 
		:overlay="false"
		:show="uviewkey"
		:tooltip="false"
		></u-keyboard>
		<!-- <CodeKeyboard ref="CodeKeyboard" passwrdType="pay" @KeyInfo="KeyInfo"></CodeKeyboard> -->
		<!-- <TabBar :tabBarShow="2"></TabBar> -->
	</view>
</template>

<script>
	import CodeKeyboard from '../../components/CodeKeyboard/CodeKeyboard.vue';
	import { createtixian,findCustomData,findConfig } from "../../api/index";
	import TabBar from '../../components/TabBar/TabBar.vue';
	export default {
		components: {
			  CodeKeyboard,
		},
		data() {
			return {
				uviewkey:false,
				timeshow:0,
				money:'',
				AffirmStatus: 1,
				passwordArr: [],
				oldPasswordArr: [],
				newPasswordArr: [],
				afPasswordArr: [],
				userinfo:'',
				starttime:'',
				endtime:'',
				config:''
			};
		},
		onLoad() {
			this.checkuserinfo()
			this.checktime()
			
		},
		onShow() {
			this.checkuserinfo()
		},
		methods:{
			checktime(){
				let _this = this
				findConfig().then((res)=>{
					let data = res.result
					this.config = res.result
					this.starttime = res.result.withdrawalStartTime
					this.endtime = res.result.withdrawalEndTime
				
					let date = new Date(parseInt(res.timestamp))
					var minute = date.getHours();
					if(res.result.withdrawalPower ==  'Y'){
							if(minute >= data.withdrawalStartTime && minute <= data.withdrawalEndTime){
								_this.timeshow = 0
							}else{
								_this.timeshow = 1
							}
					}else if(res.result.withdrawalPower == 'N'){
						_this.timeshow = 1
					}
					
				})
			},
			
			valChange(val) {
				
				// 将每次按键的值拼接到value变量中，注意+=写法
				if(val.index >= 6){
					return;
				}
				// 判断是否删除
				if(val.keyCode === 8){
					this.passwordArr.splice(val.index+1,1)
				}else{
					this.passwordArr.push(val);
				}
				// 判断是否等于6
				if(this.passwordArr.length === 6){
					this.passwordArr = [];
					this.AffirmStatus = this.AffirmStatus+1;
				}
				console.log(this.passwordArr)
				// 判断到哪一步了
				if(this.AffirmStatus === 1){
					this.oldPasswordArr = this.passwordArr;
				}else if(this.AffirmStatus === 2){
					this.newPasswordArr = this.passwordArr;
				}else{
					if(this.oldPasswordArr.join('') == this.newPasswordArr.join('')){
				
						
						let pwd = this.newPasswordArr.join('')
						
						
						console.log(pwd,this.oldPasswordArr.join(''),'支付密码',this.newPasswordArr.join(''))
						
						updateCustomData({payPwd:pwd,id:this.userinfo.id}).then((res)=>{
							if(res.code == 200){
								uni.setStorage({
								  key: 'userinfo',
								  data: res.result.psCustomMain
								});
								this.checkuserinfo()
							}
						})
						uni.showToast({
							title: '密码设置成功',
							icon: 'none'
						})
						setTimeout(() =>{
							uni.navigateBack();
						},500)
					}else{
						uni.showToast({
							title: '密码不一致',
							icon: 'none'
						})
						this.AffirmStatus = 1
				
					}
					
				
				
				
					
				}
				// this.$forceUpdate();
			},
			// 退格键被点击
			backspace() {
				// 删除value的最后一个字符
			
				if(this.passwordArr.length) this.passwordArr.splice(this.passwordArr.length-1,1)
				console.log(this.passwordArr);
			
				
			},
			/**
			 * 唤起键盘
			 */
			onPayUp(){
			
					  
					  
			},
			/**
			 * 支付键盘回调
			 * @param {Object} val
			 */
			valChange(val){
				console.log(this.money)
				
				
				this.passwordArr.push(val);

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
						this.uviewkey = false

			
							this.oldPasswordArr = []
							this.passwordArr = []
							this.AffirmStatus = 1
							
							
							createtixian({account:this.money}).then((res)=>{
								if(res.code==200){
									uni.showToast({
										title: '等待审核',
										duration: 2000,
									})
									this.checkuserinfo()
									this.money = 0
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
			tx(){
				
				if(this.userinfo.bankName==null){
					uni.showToast({
						title: '请绑定银行卡',
						icon:'error',
						duration: 2000
					});
					setTimeout(()=>{
						uni.navigateTo({
							url:'/pages/SignIn/SignIn'
						})
					},1000)
				}else if(!this.money){
					uni.showToast({
						title: '请输入金额',
						icon:'error',
						duration: 2000
					});
				}else if(this.money < this.config.withdrawalSizeMin || this.money > this.config.withdrawalSizeMax){
					uni.showToast({
						title: '提现金额不少于'+this.config.withdrawalSizeMin+'元,大于'+this.config.withdrawalSizeMax+'元',
						icon:'none',
						duration: 2000
					});
				}else if(!this.userinfo.payPwd){
					uni.showToast({
						title: '请设置支付密码',
						icon:'none',
						duration: 2000
					});
					uni.navigateTo({
						url:'/pages/PaymentPassword/PaymentPassword'
					})
				}else{
										  
					  this.$refs.popup.open('center')
					  this.uviewkey = true
				}
				
				
				
				
			},
			gosetbank(){
					uni.navigateTo({
						url:'/pages/SignIn/SignIn'
					})
			},
			checkuserinfo(){
				let _this = this
				
				findCustomData().then(res=>{
					console.log(res)
					_this.userinfo = res.result
					
					
				})
			
			},
			/**
			 * 消费明细点击
			 */
			onConsumeRecord(){
				uni.navigateTo({
					url: '/pages/ConsumeRecord/ConsumeRecord?type=2'
				})
			}
		}
	}
</script>

<style scoped lang="scss">
	@import 'cart.scss';
</style>
