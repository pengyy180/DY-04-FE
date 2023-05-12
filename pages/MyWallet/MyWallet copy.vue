<template>
	<view class="page">
		<!-- 头部背景 -->
		<view class="head-bg">
			<view class="head-user">
				<view class="user">
				</view>
				<view class="exchange" @click="onConsumeRecord">
					<text>充值记录</text>
					<text class="iconfont icon-more"></text>
				</view>
			</view>
			 <view class="wallet-balance">
				<view class="wallet">
					<text class="number">￥{{userinfo.givenAccount}}</text>
					<text><text class="iconfont icon-qianbao" style="margin-right: 20rpx;"></text>当前余额</text>
				</view>
			</view>
			<view class="bg">
				<image src="/static/integral_bg1.png" mode=""></image>
			</view>
		</view>
		<!-- 钱包充值 --> 
		<!-- <view class="wallet-recharge">
			<view class="recharge-title">
				<text>钱包充值</text>
			</view>
			<view class="recharge-list">
				<view class="list" v-for="(item,index) in moneylist" 
				:class="{action:index==selectPort}"
				:key="index"
				@click="changePort(index)">
					<view class="price">
						<text>{{item.money}}元</text>
					</view>
				</view>
				<view class="list" >
					<view class="price">

						<input type="text" v-model="pushmoney" @click="addmoney" placeholder="请输入金额" style="text-align: center;">
					</view>
				</view>
			</view>
		</view> -->
		<!-- <view class="wallet-recharge" v-if="selectPort2 == 0">
			<view class="recharge-title">
				<text>转账凭证</text>
			</view>
			<view class="imgupload" >
				<u-upload
						class="uploadimg"
				  :fileList="fileList1"
				  @afterRead="afterRead"
				  @delete="deletePic"
				  name="1"
				  multiple
				  :maxCount="1"
						width="150"
							height="150"
				></u-upload>
			</view>	
		</view> -->
		<view class="iconlist" >
			<view class="list" v-for="(item,index) in banklist"
		:class="{active:index==selectPort2}"
		:key="index"
		@click="changebank(index)">
				<image :src="item.img" mode=""></image>{{item.name}}
				
			</view>
			
		</view>
		<view class="recharge-btn" @click="apimoney">
			<!-- <view class="btn">立即充值 ￥ {{pushmoney}} </view> -->
			<view class="btn">立即充值 </view>
		</view>
		<!-- tabbar -->
		<!-- <TabBar :tabBarShow="1"></TabBar> -->
	</view>
</template>

<script>
	import { createchongzhi,findCustomData,findKefu } from "../../api/index";
	import TabBar from '../../components/TabBar/TabBar.vue';
	import __config from "../../config/env.js";
	export default {
		data() {
			return {
				basePath: __config.basePath, // 图片地址
				action: __config.action, // 图片地址
				fileList1: [],
				moneylist:[
					{money:2000},
					{money:5000},
					{money:10000},
					{money:20000},
					{money:50000},
				],
				banklist:[
					// {img:'/static/img/yhk.png',name:'银行转账'},
					// {img:'/static/img/kf.png',name:'客服转账'},
					{img:'/static/img/wx.png',name:'微信支付'},
					{img:'/static/img/zfb.png',name:'支付宝支付'}
				],
				selectPort:0,
				selectPort2:0,
				pushmoney:2000,
				money:'',
				userinfo:'',
				imgs:''
			};
		},
		onLoad() {
			this.checkuserinfo()
		},
		onShow() {
			this.checkuserinfo()
		},
		methods:{
			apimoney(){
				findKefu().then((res)=>{
					console.log(res.result)
					window.location.href="https://chatlink.mstatik.com/widget/standalone.html?eid="+res.result
				})
				
				
				
			},
			checkuserinfo(){
				let _this = this
				
				findCustomData().then(res=>{
					console.log(res)
					_this.userinfo = res.result
				})
			
			},
			addmoney(){
				this.selectPort = 99
			},
				changePort(index) {
							
							this.selectPort = index;
							this.pushmoney = this.moneylist[index].money
							
					},
					changebank(index) {
							this.selectPort2 = index;
							
							
							if(index == 0){
								uni.showToast({
									title: '支付通道维护中，请选择客服转账支付方式',
									icon: 'none'
								})
							}else if(index ==1){
								uni.showToast({
									title: '支付通道维护中，请选择客服转账支付方式',
									icon: 'none'
								})
							}if(index ==2){
								uni.showToast({
									title: '支付通道维护中，请选择客服转账支付方式',
									icon: 'none'
								})
							}
							// this.pushmoney = this.moneylist[index].money
							
					},
					// 删除图片
					deletePic(event) {
					  this[`fileList${event.name}`].splice(event.index, 1);
					},
					// 新增图片
					async afterRead(event) {
					  // 当设置 mutiple 为 true 时, file 为数组格式，否则为对象格式
					  let lists = [].concat(event.file);
					  let fileListLen = this[`fileList${event.name}`].length;
					  lists.map((item) => {
					    this[`fileList${event.name}`].push({
					      ...item,
					      status: "uploading",
					      message: "上传中",
					    });
					  });
					  for (let i = 0; i < lists.length; i++) {
					    const result = await this.uploadFilePromise(lists[i].url);
					    let item = this[`fileList${event.name}`][fileListLen];
					    this[`fileList${event.name}`].splice(
					      fileListLen,
					      1,
					      Object.assign(item, {
					        status: "success",
					        message: "",
					        url: result,
					      })
					    );
					    fileListLen++;
					  }
					  // console.log(this.fileList1, "0000000000000");
					},
					uploadFilePromise(url) {
					  return new Promise((resolve, reject) => {
					    let a = uni.uploadFile({
					      url: this.action, // 仅为示例，非真实的接口地址
					      filePath: url,
					      name: "file",
					      formData: {
					        user: "test",
					      },
					      success: (res) => {
							  
							  let data = JSON.parse(res.data)
					        this.imgs = this.basePath+'/'+data.message;

					        setTimeout(() => {
					          resolve(res.data.data);
					        }, 1000);
					      },
					    });
					  });
					},
			/**
			 * 消费明细点击
			 */
			onConsumeRecord(){
				uni.navigateTo({
					url: '/pages/ConsumeRecord/ConsumeRecord?type=1'
				})
			}
		}
	}
</script>

<style scoped lang="scss">
	@import 'MyWallet.scss';
</style>
