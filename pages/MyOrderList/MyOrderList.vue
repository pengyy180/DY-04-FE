<template>
	<view class="page">
		<view class="head-back">
			<view class="back" @click="onBack">
				<text></text>
			</view>
			<view class="title">
				<text>我的订单</text>

			</view>
			<view class="more-icon">
				<view class="icon-list">
					<text class="iconfont icon-fadajing"></text>
				</view>
			</view>
		</view>
		<!-- 订单tab -->
		<view class="order-tab">
			<view class="tab" :class="{'action':OrderType==0}" @click="onOrderTab(0)">
				<text>全部</text>
				<text class="line"></text>
			</view>
			<view class="tab" :class="{'action':OrderType==1}" @click="onOrderTab(1)">
				<text>待发货</text>
				<text class="line"></text>
			</view>
			<view class="tab" :class="{'action':OrderType==2}" @click="onOrderTab(2)">
				<text>已完成</text>
				<text class="line"></text>
			</view>
	
		</view>
		
			<u-back-top :scroll-top="scrollTop"></u-back-top>
			
		<!-- 订单列表 -->
		<view class="order-list">
			<view style="text-align: center;color: #3c3c3c73;">
				{{nowtime}}
			</view>	
			<view class="list" v-for="(item,index) in orderlist" @click="onOrderList(item)" :key="index">
				<view class="title-status">
					<view class="title">
						<text>订单时间：{{item.createTime}}</text>
					</view>
					<view class="status">
						<text v-if="item.succStatus == 2 && item.zongfanhui != 0 ">返利金额:{{item.zongfanhui}}</text>
						<!-- <text class="iconfont icon-laji del"></text> -->
					</view>
				</view>
				<view class="goods-list">
					<view class="goods">
						<view class="thumb">
							<image :src="item.productImgs" mode=""></image>
						</view>
						<view class="item">
							<view class="goods-name">
								<text class="two-omit">{{item.productName}}</text>
							</view>
							<view class="goods-price">
								<text class="min">￥</text>
								<text class="max">{{item.productPrice}}</text>
								<text class="min">.00</text>
							</view>
							
						</view>
					</view>
				</view>
				<view class="status-btn">
					<view class="btn">
						<text>订单编号:{{item.id}} </text>
					</view>
					
					<!-- <view class="btn "  v-if="item.zongfanhui > 0" style="color: darkred; padding-bottom: 10rpx;">
						<text  style="color: darkred; ">返利金额:{{item.zongfanhui}}</text>
					</view> -->
				</view>

			</view>

			<uni-load-more :status="status"></uni-load-more>
		</view>
	</view>
</template>

<script>
	import { orderList,orderList2 } from "../../api/index";
	import __config from "../../config/env.js";
	export default {
		data() {
			return {
				scrollTop: 0 ,
				action: __config.basePath, // 图片显示地址
					OrderType: 0,
				orderlist:[],
				status: 'none',
				pagesize:10,
				pageNo:1,
				nowtime:''
				
			};
		},

		onLoad(params) {
			this.OrderType = params.type;
			this.checklist(params.type)
		},
		 // onPullDownRefresh() {
		 //            //下拉刷新
					
			// 		uni.$u.toast("已完成刷新操作")
			// 			this.checklist(this.OrderType)


			// 	},
				onReachBottom() {
					// this.pageNo +=1
					// this.checklist(this.OrderType)
					// uni.$u.toast("上拉触发",this.pageNo)
					//这是上拉触发的函数.一般在这里进行分页操作. 
		            /*会有一种情况就是当数据已经全部加载完成了,再次触底还会调用接口,
		              这时就需要去记录一下是否已经全部加载完毕,加载完毕就不去请求接口了.
		              这时最常见的节流场景*/
				},
		methods:{
			async checklist(index){
				// const  res  = await orderList({orderStatus:index,pageNo:this.pageNo,pagesize:this.pagesize})
				const  res  = await orderList2()
				this.nowtime = 		new Date(parseInt(res.timestamp)).toLocaleString()
					// this.orderlist = res.result	
					for (let i = 0; i < res.result.length; i++) {
						let imgs = res.result[i].productImgs.split(",")
						res.result[i].productImgs = this.action + '/' +imgs[0]

					}
					
					let data = res.result
					if(index == 0){
						this.orderlist = data
					}else if(index == 1){
						data.forEach((e)=>{
							if(e.orderStatus == 1){
								console.log(e.orderStatus,'1111')
								this.orderlist.push(e)
							}
						})
					}else if(index == 2){
						data.forEach((e)=>{
							if(e.orderStatus == 2){
								console.log(e.orderStatus,'2222')
								this.orderlist.push(e)
							}
						})
					}
					
					
					// this.orderlist = this.orderlist.concat(res.result.records)
				// console.log(res)
				
			},
			/**
			 * 返回点击
			 */
			onBack(){
				uni.navigateBack();
			},
			/**
			 * 订单tab点击
			 */
			onOrderTab(type){
				console.log(type)
				this.pageNo = 1
				this.pagesize = 10
				this.OrderType = type;
				this.orderlist = []
				this.checklist(type)
				uni.pageScrollTo({
					scrollTop: 0,
					duration: 1000
				});
				// // #ifdef H5
				// uni.redirectTo({
				// 	url: '/pages/MyOrderList/MyOrderList?type=' + type,
				// })
				// //#endif
			},
			/**
			 * 订单列表点击
			 */
			onOrderList(item){
				uni.navigateTo({
					url: '/pages/OrderDetails/OrderDetails?id='+item.id,
				})
			},
      /**
       * 评价点击
       */
      onEvaluate(){
			  uni.navigateTo({
          url: '/pages/MyEvaluatePush/MyEvaluatePush'
        })
      }
		}
	}
</script>

<style scoped lang="scss">
	@import 'MyOrderList.scss';
</style>
