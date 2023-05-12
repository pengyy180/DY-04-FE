<template>
	<view class="page">
	
		<view class="setting-password" >
			<view class="inpu-password">
				<text>修改手机号码</text>
				<input type="text" v-model="customPhoneNum" placeholder="请输入要修改手机号码" />

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
				customPhoneNum:''
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
				
				updateCustomData({customPhoneNum:this.customPhoneNum,id:this.userinfo.id}).then((res)=>{
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
	@import 'ModifyPhone.scss';
</style>
