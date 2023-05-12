<template>
	<view class="page">
		<!-- 搜索 -->
		<view class="search-head">
			<view class="back" @click="onBack">
				<text></text>
			</view>
			<view class="search" >
				<text class="iconfont icon-fadajing"></text>
				<input type="text" v-model="keyword" @confirm="searchtext" placeholder="搜索商品" />
			</view>
			<!-- <view class="cut" @click="isList = !isList">
				<text class="iconfont" :class="isList?'icon-shitu01':'icon-shitu02'"></text>
			</view> -->
		</view>
		
		<!-- 商品列表 -->
		<view class="goods-data">
				<view class="procat">
					<view class="protitle">
						<!-- {{typename}} -->
					</view>
					<view class="list" >
						<view class="li3" v-for="(item, index) in goodsList" @click="onGoodsList(item)">
							<image class="li-img" :src="item.productImgs" ></image>
							<text class="title">
								{{item.productTitle}}
							</text>
							<text class="price">￥{{item.productPrice}}</text>
							<view class="cart">
								<text class="iconfont icon-cart"></text>立即购买
							</view>
						</view>
					</view>
				</view>  
				<!-- <view class="goods-list">
					<view :class="isList?'list-view':'list-li'" v-for="(item,index) in goodsList" @click="onGoodsList(item)" :key="index">
						<view class="thumb">
							<image :src="item.productImgs" mode="heightFix"></image>
						</view>
						<view class="item">
							<view class="title">
								<text class="two-omit">{{item.productTitle}}</text>
							</view>
							<view class="price">
								<view class="retail-price">
									<text class="min">￥</text>
									<text class="max">{{item.productPrice}}</text>
									<view class="tag" v-if="item.is_goods === 1">
										<text>抢购价</text>
									</view>
								</view>
								<view class="vip-price">
									<text class="min">￥</text>
									<text class="max">{{item.productPrice}}</text>
								</view>
							</view>
						</view>
					</view>
				</view> -->
			<!-- </mescroll-body> -->
		</view>
		<!-- 抽屉 -->
		
	</view>
</template>

<script>
	// 引入mescroll-mixins.js
	
	import { findProductList } from "../../api/index";
	// import MescrollMixin from "@/components/mescroll-uni/mescroll-mixins.js";
	import __config from "../../config/env.js";
	export default {
		// mixins: [MescrollMixin], // 使用mixin
		data() {
			return {
				action: __config.imgaction, // 图片显示地址
				mescroll: null, // mescroll实例对象 (此行可删,mixins已默认)
				// 下拉刷新的配置(可选, 绝大部分情况无需配置)
				downOption: {},
				// 上拉加载的配置(可选, 绝大部分情况无需配置)
				upOption: {
				},
				// 列表视图切换
				isList: true,
				// 筛选弹窗
				isScreen: false,
				// 筛选切换
				screenShow: 0,
				// 抽屉
				isDrawer: false,
				keyword: '',
        goodsList:[],
			}
		},
		onLoad(params) {
			this.keyword = decodeURIComponent(params.keyword||'');
			
			console.log(this.keyword)
			this.funFindProductList(this.keyword)
		},
		methods: {
			searchtext(){

				this.funFindProductList(this.keyword)
			},
			//   商品列表
			funFindProductList(productName) {
			  findProductList({productName:productName,pageNo:1,pageSize:20}).then((res) => {
			    // this.goodsList = res.result;
			
			    for (let i = 0; i < res.result.records.length; i++) {
			    	
			    	let imglist = res.result.records[i].productImgs.split(",")
			    	res.result.records[i].productImgs = this.action + '/' +imglist[0]
			    }
				this.goodsList = res.result.records
			  });
			},
			/*下拉刷新的回调, 有三种处理方式:*/
			downCallback(){
				this.mescroll.endSuccess();
			},
			/*上拉加载的回调*/
			upCallback(page) {
				setTimeout(() =>{
					this.mescroll.endByPage(10, 20);
				},2000)
			},
			/**
			 * 返回点击
			 */
			onBack(){
				uni.navigateBack();
			},
			/**
			 * 综合点击
			 * @param {Number} type
			 */
			onSynthesize(type){
				this.screenShow = type;
				this.isScreen = !this.isScreen;
			},
			/**
			 * 商品列表点击
			 */
			onGoodsList(item){
				console.log(item)
				uni.navigateTo({
					url: '/pages/GoodsDetails/GoodsDetails?id='+item.id,
					animationType: 'zoom-fade-out',
					animationDuration: 200
				})
			}
		}
	}
</script>

<style scoped lang="scss">
	@import  'SearchGoodsList.scss';
</style>
