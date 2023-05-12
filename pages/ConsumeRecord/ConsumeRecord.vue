<template>
	<view class="page">
		<!-- 头部背景 -->
		<!-- <view class="head-bg">
			<view class="wallet-balance">
				<view class="wallet">
					<text class="number">￥{{userinfo.givenAccount}}</text>
					<text><text class="iconfont icon-qianbao" style="margin-right: 20rpx;"></text>当前余额</text>
					<text>{{nowtime}}</text>
				</view>
			</view>
			<view class="bg">
				<image src="/static/integral_bg1.png" mode=""></image>
			</view>
		</view> -->
		<view style="width: 100%;height: 20px;"></view>	
		<view class="tablist">
			<view class="list" v-for="(item,index) in moneytype"
			:class="{action:index==selectPort}"
			:key="index"
			@click="changePort(index)">
				<view class="price">
					<text>{{item.name}}</text>
				</view>
			</view>
		</view>
		<!-- 记录列表 -->
		<view class="record-list">
			<view class="list" v-for="(item,index) in moneylist" :key="index" @click="urldetails(item.id)">
				<view class="icon">
					<text class="cuIcon-moneybag" ></text>
				</view>	
				<view class="title-date">
					<view class="title">
						<text  v-if="item.accountStatus==0" class="status-1">{{item.accountStatusname}}:<text class="colorb">{{item.accountTitle}}:{{item.accountBigdec}}元</text></text>
						<text v-if="item.accountStatus==1" class="status-2">{{item.accountStatusname}}:<text class="colorb">{{item.accountTitle}}:{{item.accountBigdec}}元</text></text>
						<text v-if="item.accountStatus==2" class="status-3">{{item.accountStatusname}}:<text class="colorb">{{item.accountTitle}}:{{item.accountBigdec}}元</text></text>

						<!-- <text>状态:{{item.accountTitle}}:{{item.accountBigdec}}元  </text> -->
					</view>
					<view class="date">
						<text>流水号:{{item.id}} </text>
					</view>
					<view class="date">
						<text>创建时间: {{item.createTime}} </text>
					</view>
				</view>
				<view class="integral">
					<text v-if="item.accountType== 1 || item.accountType== 4 || item.accountType== 5" style="color: #22AA44;">+￥{{item.accountBigdec}}</text>
					<text v-else>-￥{{item.accountBigdec}}</text>
				</view>
			</view>
		</view>
		<uni-load-more :status="status"></uni-load-more>
	</view>
</template>

<script>
	
	import { findtixianList,findtixianLis2,findCustomData } from "../../api/index";
	export default {
		data() {
			return {
				scrollTop: 0 ,
				status: 'none',
				userinfo:[],
				moneylist:[],
				moneytype:[
					{name:'余额明细'},
					{name:'充值'},
					{name:'提现'},
				],
				selectPort:0,
				pagesize:10,
				pageNo:1,
				nowtime:''
				
			};
		},
		onLoad(option) {
			this.checkuserinfo()
			this.checklist(option.type)
			this.selectPort = option.type
		},
			// onShow() {
			// 	this.checkuserinfo()
			// 	this.checklist(0)
			// },
		onReachBottom() {
			// this.pageNo +=1
			// this.checklist(this.selectPort)
			// uni.$u.toast("上拉触发",this.pageNo)
			//这是上拉触发的函数.一般在这里进行分页操作. 
		    /*会有一种情况就是当数据已经全部加载完成了,再次触底还会调用接口,
		      这时就需要去记录一下是否已经全部加载完毕,加载完毕就不去请求接口了.
		      这时最常见的节流场景*/
		},
		methods:{
			urldetails(id){
				uni.navigateTo({
					url:'/pages/OrderDetails2/OrderDetails2?id='+id
				})
			},
			changePort(index) {
						
						this.selectPort = index;
						this.pageNo = 1
						this.pagesize = 10
						this.moneylist = []
						// this.pushmoney = this.moneylist[index].money
						this.checklist(index)
						uni.pageScrollTo({
							scrollTop: 0,
							duration: 1000
						});
				},
			checkuserinfo(){
				let _this = this
				
				findCustomData().then(res=>{

					_this.userinfo = res.result
				})
			
			},
			async checklist(index){
				// const  res  = await findtixianList({payType:index,pageNo:this.pageNo,pagesize:this.pagesize})
				const  res  = await findtixianLis2()
					
				this.nowtime = 		new Date(parseInt(res.timestamp)).toLocaleString()
				console.log(res)
					// accountStatus
					res.result.forEach((e)=>{
						if(e.accountStatus == 0){
							e.accountStatusname = '申请中'
						}else if(e.accountStatus == 1){
							e.accountStatusname = '通过'
						}else if(e.accountStatus == 2){
							e.accountStatusname = '拒绝'
						}
						
						
						if(e.accountType == 1){
							e.accountTypename = '充值'
						}else if(e.accountType == 2){
							e.accountTypename = '提现'
						}else if(e.accountType == 3){
							e.accountTypename = '消费'
						}else if(e.accountType == 4){
							e.accountTypename = '返利'
						}else if(e.accountType == 5){
							e.accountTypename = '充值'
						}
					})
					let data = res.result
					if(index == 0){
						
						data.forEach((e)=>{
							if(e.accountType == 3){
								console.log(e.accountType,'1111')
								this.moneylist.push(e)
							}
						})
					}else if(index == 1){
						data.forEach((e)=>{
							if(e.accountType == 1 ||  e.accountType == 5){
								console.log(e.accountType,'1111')
								this.moneylist.push(e)
							}
						})
					}else if(index == 2){
						data.forEach((e)=>{
							if(e.accountType == 2){
								console.log(e.accountType,'2222')
								this.moneylist.push(e)
							}
						})
					}
				
			}
		}
	}
</script>

<style scoped lang="scss">
	@import 'ConsumeRecord.scss';
</style>
