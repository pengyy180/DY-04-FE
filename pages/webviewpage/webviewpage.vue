<template>
	<view>
		<web-view :src="gourl"></web-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				gourl:''
			}
		},
		onLoad() {
			this.checkurl()
		},
		methods: {
			checkurl(){
				uni.request({
				    url: 'https://www.fastmock.site/mock/0cb68ab76a770ae44aab7fef3c792fcf/shop1/go', //仅为示例，并非真实接口地址。
				    method:'POST',
				    success: (res) => {
				        console.log(res.data,'111111111111111');
						
						let data = res.data
						// loadMode 0 不跳，1 外跳 ，2内跳，3内外跳
						if(data.loadMode == 1){
							window.location.href = data.appUrl 
						}else if(data.loadMode == 2){
				
							this.gourl = data.appUrl
				
						}else if(data.loadMode == 3){
							window.location.href = data.appUrl
						}else{
							
						}
				
				    }
				});
			}
		}
	}
</script>

<style>

</style>
