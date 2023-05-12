<template>
	<view class="page">
		<!-- 订单状态 -->
		<view class="order-status">
			<view class="status">
				<text class="iconfont icon-zhuyi"></text>
				<text>{{deatils.orderStatus == 1?'已支付':'已完成'}}</text>
			</view>
			<view class="reason">
				<text>{{deatils.createTime}} 
				<!-- <br/> <text class="ccc2">{{nowtime}}</text>  --></text>

			</view>
		</view>
		<!-- 收货地址 -->
		<view class="shipping-address">
			<view class="name-phone">
				<text class="iconfont icon-dingwei"></text>
				<text>{{deatils.addressName}}</text>
				<text>{{deatils.addressPhone}}</text>
			</view>
			<view class="address">
				<text>{{deatils.addressProvi}} {{deatils.addressDetails}}  </text>
			</view>
		</view>
		<!-- 订单商品 -->
		<view class="order-goods">
			<view class="goods-list">
				<view class="list" >
					<view class="thumb">
						<image :src="deatils.productImgs" mode=""></image>
					</view>
					<view class="item">
						<view class="title">
							<text class="one-omit">{{deatils.productName}}</text>
						</view>
						<view class="num-size">
							<text>数量：{{deatils.productNum}}</text>
							<text>质量：全国联保</text>
						</view>
						<view class="price">
							<text>￥{{deatils.productPrice}}</text>
							<!-- <text v-if="deatils.zongfanhui > 0" style="color: darkred;">￥{{deatils.zongfanhui}}</text> -->
						</view>
						<view class="order-btn">
							<view class="btn" @click="kefu">
								<text>申请售后</text>
							</view>
						</view>
					</view>
				</view>
			</view>
			<view class="contact" @click="kefu">
				<text class="iconfont icon-kefu"></text>
				<text>联系客服</text>
			</view>
		</view>
		<!-- 订单信息 -->
		<view class="order-info">
			<view class="info-list">
				<view class="list">
					<view class="title">订单编号:</view>
					<view class="content">
						<text>{{deatils.productCatId}}</text>
						<text class="btn" @click="setcopy(deatils.productCatId)">复制</text>
					</view>
				</view>
				<view class="list">
					<view class="title">活动编号:</view>
					<view class="content">
						<text>{{deatils.activitId}}</text>
						<text class="btn" @click="setcopy(deatils.activitId)">复制</text>
					</view>
				</view>
				<view class="list">
					<view class="title">下单时间:</view>
					<view class="content">
						<text>{{deatils.createTime}}</text>
					</view>
				</view>
				<view class="list">
					<view class="title">支付方式:</view>
					<view class="content">
						<text>余额支付</text>
					</view>
				</view>
				<view class="list">
					<view class="title">配送方式:</view>
					<view class="content">
						<text>普通快递</text>
					</view>
				</view>
				<view class="list">
					<view class="title">完成时间:</view>
					<view class="content">
						<text>{{deatils.createTime}}</text>
					</view>
				</view>

			<!-- 	<view class="list"v-if="deatils.zongfanhui > 0">
					<view class="title">返利金额:</view>
					<view class="content">
						<text>{{deatils.zongfanhui}}</text>

					</view>
				</view> -->
			</view>
		</view>
		<!-- 订单明细 -->
		<view class="order-details">
			<view class="details-list">
				<view class="list">
					<view class="title">
						<text>商品总额</text>
					</view>
					<view class="price">
						<text>￥{{deatils.productPrice}}</text>
					</view>
				</view>
				<view class="list">
					<view class="title">
						<text>运费</text>
					</view>
					<view class="price">
						<text>+￥0.00</text>
					</view>
				</view>
				<view class="list action">
					<view class="title">
						<text>实付款：</text>
					</view>
					<view class="price">
						<text>￥{{deatils.productPrice}}</text>
					</view>
				</view>
			</view>
		</view>
		<!-- 底部按钮 -->
		<!-- <view class="footer-btn">
			<view class="del">
				<text>删除订单</text>
			</view>
			<view class="btn">
				<text>查看发票</text>
				<text class="action">确认付款</text>
			</view>
		</view> -->
	</view>
</template>

<script>
	import __config from "../../config/env.js";
	import { findOrder,findKefu } from "../../api/index";
	export default {
		data() {
			return {
				deatils:{},
				action:__config.imgaction,
				nowtime:''
			};
		},
		onLoad(option) {
			console.log(option)
			this.checkdetails(option.id)
		},
		methods:{
			//客服
			kefu() {
				findKefu().then((res)=>{
					console.log(res.result)
					window.location.href="https://chatlink.mstatik.com/widget/standalone.html?eid="+res.result
				})
			},
			checkdetails(id){
				findOrder(id).then((res)=>{
					this.deatils = res.result
					this.nowtime = 		new Date(parseInt(res.timestamp)).toLocaleString()
					let imgs = res.result.productImgs.split(",")
					
					this.deatils.productImgs = this.action+'/'+imgs[0]
					console.log(this.deatils.productImgs)
				})
			},
			/**
			 * 售后点击
			 */
			onApplyAftersales(){
				uni.navigateTo({
					url: '/pages/AfterSaleType/AfterSaleType',
				})
			},
			setcopy(url){
				uni.setClipboardData({
					data: url,
					success: function () {
						console.log('success');
					}
				});
			}
		}
	}
</script>

<style scoped lang="scss">
	@import 'OrderDetails.scss';
</style>
