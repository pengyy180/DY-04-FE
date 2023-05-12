<template>
	<view class="page">
		<!-- 安全设置列表 -->
		<view class="security-list">
			<view class="list" @click="onSecurity('relevance')">
				<view class="content">
					<view class="title">
						<text>账号关联</text>
					</view>
					<view class="describe">
						<text>可用使用微信快速登录</text>
					</view>
				</view>
				<view class="more">
					<text class="iconfont icon-more"></text>
				</view>
			</view>
			<view class="list" @click="onSecurity('password')">
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
			<view class="list" @click="onSecurity('phone')">
				<view class="content">
					<view class="title">
						<text>修改手机号</text>
					</view>
					<view class="describe">
						<text>若手机更换请尽快修改</text>
					</view>
				</view>
				<view class="more">
					<text>188****8888</text>
					<text class="iconfont icon-more"></text>
				</view>
			</view>
			<view class="list" @click="onSecurity('mailbox')">
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
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			};
		},
		methods:{
			/**
			 * 账户安全点击
			 * @param {String} type
			 */
			onSecurity(type){
				switch(type) {
					case 'relevance':
						uni.navigateTo({
							url: '/pages/AccountAssociated/AccountAssociated',
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
			 * 申诉列表点击
			 * @param {String} tyep
			 */
			onAppeal(tyep){
				switch (type){
					case 'appeal':
						
						break;
				}
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import 'AccountSecurity.scss';
</style>
