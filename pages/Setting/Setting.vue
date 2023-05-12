<template>
	<view class="page">
		<!-- 用户信息 -->
		<!-- <view class="user-info">
			<view class="user-data" @click="onUserInfo">
				<view class="portrait-nickname">
					<view class="portrait">
						<image src="/static/img/user_pic.jpg" mode=""></image>
					</view>
					<view class="nickname">
						<text>爱跳舞的汤姆猫</text>
					</view>
				</view>
				<view class="more">
					<text class="iconfont icon-more"></text>
				</view>
			</view>

		</view> -->
		<!-- 用户信息列表 -->
		<view class="user-list">
			<view class="list" style="height: 160rpx;" @click="uploadimg">
				<view class="title">
					<text>头像</text>
				</view>
				<view class="more-content">
					<image :src="userinfo.headPortrait" mode=""></image>
					<text class="iconfont icon-more more"></text>
				</view>
			</view>
			<view class="list" @click="onNickname">
				<view class="title">
					<text>昵称</text>
				</view>
				<view class="more-content">
					<text class="content">{{userinfo.customName}}</text>
					<text class="iconfont icon-more more"></text>
				</view>
			</view>
			<view class="list">
				<view class="title">
					<text>性别</text>
				</view>
				<view class="more-content">
					<text class="content">{{userinfo.customSex==1?'男':'女'}}</text>
					<text class="iconfont icon-more more"></text>
				</view>
				<view class="picker">
					<picker @change="sexPickerChange" :value="sexIndex" :range="sexArray">
						<view class="uni-input" style="height: 100rpx;">{{sexText}}</view>
					</picker>
				</view>
			</view>
			<view class="list">
				<view class="title">
					<text>出生日期</text>
				</view>
				<view class="more-content">
					<text class="content">{{userinfo.customBirth}}</text>
					<text class="iconfont icon-more more"></text>
				</view>
				<view class="picker">
					<picker @change="birthdayPickerChange" mode="date" :value="birthdayDate" :start="startDate" :end="endDate">
						<view class="uni-input" style="height: 100rpx;">{{userinfo.customBirth}}</view>
					</picker>
				</view>
			</view>
		</view>
		<!-- 提示框 -->
		<DialogBox ref="DialogBox"></DialogBox>
		<!-- 设置列表 -->
		<view class="security-list">
			
			<view class="list" @click="onAddress">
				<view class="content">
					<view class="title">
						<text>地址管理</text>
					</view>
					<view class="describe">
						<text>设置收货地址，顺丰包邮到家</text>
					</view>
				</view>
				<view class="more">
					<text class="iconfont icon-more"></text>
				</view>
			</view>
			<view class="list" @click="onSetting('pay')">
				<view class="content">
					<view class="title">
						<text>支付设置</text>
					</view>
					<view class="describe">
						<text>设置支付密码，购买更安全放心</text>
					</view>
				</view>
				<view class="more">
					<text class="iconfont icon-more"></text>
				</view>
			</view>
			<view class="list" @click="onSetting('password')">
				<view class="content">
					<view class="title">
						<text>修改登录密码</text>
					</view>
					<view class="describe">
						<text>建议您定期更改密码以保护账户安全</text>
					</view>
				</view>
				<view class="more">
					<text class="iconfont icon-more"></text>
				</view>
			</view>
			<view class="list" @click="onSetting('phone')">
				<view class="content">
					<view class="title">
						<text>修改手机号</text>
					</view>
					<view class="describe">
						<text>若手机更换请尽快修改</text>
					</view>
				</view>
				<view class="more">
					<text class="iconfont icon-more"></text>
				</view>
			</view>
			<view class="list" @click="onSetting('mailbox')">
				<view class="content">
					<view class="title">
						<text>关联邮箱</text>
					</view>
					<view class="describe">
						<text>可用于账号登录/身份认证等</text>
					</view>
				</view>
				<view class="more">
					<text class="iconfont icon-more"></text>
				</view>
			</view>
			<view style="width: 100%;height: 150rpx;"></view>
			
			<!-- <view class="list" @click="onSetting('invoice')">
				<view class="title">
					<text>发票</text>
				</view>
				<view class="more-content">
					<text class="content">添加发票</text>
					<text class="iconfont icon-more more"></text>
				</view>
			</view> -->
			<!-- <view class="list" @click="onSetting('vip')">
				<view class="title">
					<text>商城会员</text>
				</view>
				<view class="more-content">
					<text class="content">会员专属商品</text>
					<text class="iconfont icon-more more"></text>
				</view>
			</view> -->
		</view>
		<!-- 设置列表 -->
		<view class="setting-list">
			<!-- #ifndef H5 -->
				<view class="list" @click="onSetting('common')">
					<view class="title">
						<text>通用</text>
					</view>
					<view class="more-content">
						<text class="content">清除本地缓存等</text>
						<text class="iconfont icon-more more"></text>
					</view>
				</view>
			<!-- #endif -->
			<!-- <view class="list" @click="onSetting('about')">
				<view class="title">
					<text>关于我们</text>
				</view>
				<view class="more-content">
					<text class="content"></text>
					<text class="iconfont icon-more more"></text>
				</view>
			</view> -->
		</view>
		<!-- 退出 -->
		<view class="quit-login" @click="onQuitLogin">
			<text>退出登录</text>
		</view>
		<!-- 提示框 -->
		<DialogBox ref="DialogBox"></DialogBox>
	</view>
</template>

<script>
	import { updateCustomData,findCustomData } from "../../api/index";
	import __config from "../../config/env.js";
	export default {
		data() {
			const currentDate = this.getDate({
					format: true
			})
			return {
				// 性别
				action:__config.imgaction,
				imgaction:__config.basePath,
				sexArray: ['男','女'],
				sexIndex: 1,
				sexText: '男',
				// 生日
				birthdayDate: currentDate,
				startDate: this.getDate('start'),
				endDate: this.getDate('end'),
				birthday: '',
				DialogBox: {},
				// 昵称
				nickname: '',
				userinfo:''
			};
		},
		onLoad() {
				this.checkuserinfo()
		},
		methods:{
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
			checkuserinfo(){
				let _this = this
				
				findCustomData().then(res=>{
					console.log(res)
					_this.userinfo = res.result
					_this.userinfo.headPortrait = _this.imgaction+'/'+res.result.headPortrait
				})

			},
			/**
			 * 性别
			 * @param {Object} e
			 */
			sexPickerChange(e){
				let customSex = 1
				if(e.detail.value == 0){
					customSex = 1
				}else if(e.detail.value == 1){
					customSex = 2
				}
				console.log(customSex)
				updateCustomData({customSex:customSex,id:this.userinfo.id}).then((res)=>{
					if(res.code == 200){
						uni.setStorage({
						  key: 'userinfo',
						  data: res.result.psCustomMain
						});
						this.checkuserinfo()
					}
				})
				this.sexIndex = e.detail.value;
				this.sexText = this.sexArray[this.sexIndex];
			},
			/**
			 * 生日
			 * @param {Object} e
			 */
			birthdayPickerChange(e){
				
				updateCustomData({customBirth:e.detail.value,id:this.userinfo.id}).then((res)=>{
					if(res.code == 200){
						uni.setStorage({
						  key: 'userinfo',
						  data: res.result.psCustomMain
						});
						this.checkuserinfo()
					}
				})
				

			},
			/**
			 * 获取日期
			 * @param {Object} type
			 */
			getDate(type) {
				const date = new Date();
				let year = date.getFullYear();
				let month = date.getMonth() + 1;
				let day = date.getDate();
			
				if (type === 'start') {
						year = year - 60;
				} else if (type === 'end') {
						year = year + 2;
				}
				month = month > 9 ? month : '0' + month;;
				day = day > 9 ? day : '0' + day;
				return `${year}-${month}-${day}`;
			},
			/**
			 * 昵称点击
			 */
			onNickname(){
				this.$refs['DialogBox'].confirm({
					title: '更改昵称',
					placeholder: '请输入修改的昵称',
					value: this.nickname,
					DialogType: 'input',
					animation: 0
				}).then((res)=>{
					
					updateCustomData({customName:res.value,id:this.userinfo.id}).then((res)=>{
						if(res.code == 200){
							uni.setStorage({
							  key: 'userinfo',
							  data: res.result.psCustomMain
							});
							this.checkuserinfo()
						}
					})
					this.nickname = res.value;
				})
			},
			/**
			 * 用户信息点击
			 */
			onUserInfo(){
				uni.navigateTo({
					url: '/pages/Information/Information'
				})
			},
			/**
			 * 地址点击
			 */
			onAddress(){
				uni.navigateTo({
					url: '/pages/AddressEdit/AddressEdit?type=1',
				})
			},
			/**
			 * 设置列表点击
			 * @param {String} type
			 */
			onSetting(type){
				switch(type) {
					case 'account':
						uni.navigateTo({
							url: '/pages/AccountSecurity/AccountSecurity'
						})
						break;
					case 'pay':
						uni.navigateTo({
							url: '/pages/PaymentPassword/PaymentPassword'
						})
						break;
					case 'invoice':
						uni.navigateTo({
							url: '/pages/InvoiceList/InvoiceList'
						})
						break;
					case 'vip':
						uni.navigateTo({
							url: '/pages/MyMemberInterest/MyMemberInterest'
						})
						break;
					case 'common':
						uni.navigateTo({
							url: '/pages/SettingCommon/SettingCommon'
						})
						break;
					case 'about':
						uni.navigateTo({
							url: '/pages/AboutUs/AboutUs'
						})
						break;
					case 'password':
						uni.navigateTo({
							url: '/pages/ChangePassword/ChangePassword',
						})
						break;
					case 'phone':
						uni.navigateTo({
							url: '/pages/ModifyPhone/ModifyPhone',
						})
						break;
					case 'mailbox':
						uni.navigateTo({
							url: '/pages/AssociatedmMailbox/AssociatedmMailbox',
						})
						break;
				}
			},
			/**
			 * 退出点击
			 */
			onQuitLogin(){
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
			}
		}
	}
</script>

<style scoped lang="scss">
	@import 'Setting.scss';
</style>
