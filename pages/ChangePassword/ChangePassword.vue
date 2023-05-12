<template>
	<view class="page">
	
		<view class="setting-password" >
			<view class="inpu-password">
				<text>设置密码</text>
				<input type="password" v-model="customPwd" placeholder="请输入要设置的密码" />
				<text class="iconfont icon-eye-on icon"></text>
			</view>
			<view class="btn" @click="savepwd">
				<text>确认</text>
			</view>
		</view>
	</view>
</template>

<script>
	import { updateCustomData,findCustomData } from "../../api/index";
	export default {
		data() {
			return {
				isSetting: false,
				userinfo:[],
				customPwd:''
			};
		},
		onLoad() {
				this.checkuserinfo()
		},
		methods:{
			checkuserinfo(){
				let _this = this
				
				findCustomData().then(res=>{
					console.log(res)
					_this.userinfo = res.result
				})
			
			},
			savepwd(){
				
				updateCustomData({customPwd:this.customPwd,id:this.userinfo.id}).then((res)=>{
					if(res.code == 200){
						uni.setStorage({
						  key: 'userinfo',
						  data: res.result.psCustomMain
						});
						this.checkuserinfo()
						uni.showToast({
							title: '修改成功',
							icon: 'none'
						})
					}
				})
				setTimeout(() =>{
					uni.navigateBack();
				},2000)
			}
		}
	}
</script>

<style scoped lang="scss">
	@import 'ChangePassword.scss';
</style>
