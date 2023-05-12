<template>
	<view class="page">
		<!-- 头部背景 -->
		<view class="head-info ">
			<view class="head-top">
				<text class="cuIcon-selection"></text> 资金安全有保障
			</view>	
			<view class="title">
				<view >可用余额(元)
					<text @click="moneyshow = true" v-if="moneyshow == false" class="iconfont icon-eye-off"></text> 
					<text @click="moneyshow = false" v-if="moneyshow == true" class="iconfont icon-eye-on"></text> 
				</view>
				
			</view>
			<view class="money">
				<!-- <view class="yen">
					总余额
				</view>	 -->
				<view class="number">
					
					<text v-if="moneyshow == true"><text class="sm">￥</text>{{userinfo.givenAccount}}</text>
					<text v-if="moneyshow == false">****</text>
				</view>
			</view>
			<view class="btnlist">
				<view class="mbtn" @click="goto('/pages/cart/cart')">
					前往提现
				</view>
				<view class="mbtn btnc1" @click="apimoney">
					立即充值
				</view>
			</view>
			<view class="moretext" >
				<text @click="checki = true" v-if="checki == false" class="iconfont icon-checked"></text>
				<text @click="checki = false" v-if="checki == true" class="iconfont icon-check"></text>	
				<text @click="goto('/pages/PaymentCode/PaymentCode')">钱包服务协议</text>
			</view>
		</view>
		
		<!-- <view class="head-info">
			<view class="title">
				充值方式
			</view>
			<view class="iconlist" >
				<view class="list" v-for="(item,index) in banklist"
			:class="{active:index==selectPort2}"
			:key="index"
			@click="changebank(index)">
					<image :src="item.img" mode=""></image>{{item.name}}
					
				</view>
				
			</view>
			
		</view> -->
		<!-- <view class="head-info" style="margin-top: 20rpx;">
			<view class="title">
				在线充值
			</view>
			<view class="iconlist" >
				<view class="list" v-for="(item,index) in banklist"
			:class="{active:index==selectPort2}"
			:key="index"
			@click="changebank(index)">
					<image :src="item.img" mode=""></image>{{item.name}}
					
				</view>
				
			</view>
			<view class="gochongzhi" @click="apimoney">
				<view>立即充值</view>
				<text>并同意钱包服务协议</text>
			</view>
			
		</view> -->
		<view class="content-info">
			<view class="title">
				账单管理
			</view>
			<view class="right ">
				
				
				<view class="bottom bg3" @click="goto('/pages/ConsumeRecord/ConsumeRecord?type=1')">
					<view class="dfcont">
						<text class="iiconfont cuIcon-edit color2"></text>
						<view class="more">
							<view>充值记录</view>
						</view>
					</view>
					
				</view>
				<view class="top bg2" @click="goto('/pages/ConsumeRecord/ConsumeRecord?type=2')">
					<view class="dfcont">
						<text class="iiconfont cuIcon-calendar color2"></text>
						<view class="more">
							<view>提现记录</view>
				
						</view>
					</view>
				
				</view>
				<view class="top bg2" @click="goto('/pages/ConsumeRecord/ConsumeRecord?type=0')">
					<view class="dfcont">
						<text class="iiconfont cuIcon-text color2"></text>
					<view class="more">
							<view>消费记录</view>
				
						</view>
					</view>
					
				</view>
			</view>
		</view>
		<view class="zdlist">
			<view class="title">
				<view>余额变动明细</view>
				<view  @click="goto('/pages/ConsumeRecord/ConsumeRecord?type=0')">全部 <text class="cuIcon-right"></text> </view>
			</view>
			<view class="list" v-for="(item,index) in yelist.slice(0,5)" :key="index">
				<view class="li">
					<view class="left">
						<view >
							{{item.accountTitle}}:{{item.accountBigdec}}元
						</view>
						<view >
							{{item.createTime}}
						</view>
					</view>
					<view class="right">
						<view >
							<text v-if="item.accountType== 1 || item.accountType== 4 || item.accountType== 5" style="color: #22AA44;">+￥{{item.accountBigdec}}</text>
							<text v-else>-￥{{item.accountBigdec}}</text>
						</view>
						<!-- <view >
							余额:{{item.accountBigdec}}
						</view> -->
					</view>
				</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	import { createchongzhi,findCustomData,findKefu,findtixianLis2 } from "../../api/index";
	import TabBar from '../../components/TabBar/TabBar.vue';
	import __config from "../../config/env.js";
	export default {
		data() {
			return {
				basePath: __config.imgaction, // 图片地址
				action: __config.action, // 图片地址
				fileList1: [],
				moneylist:[],
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
				imgs:'',
				yelist:[],
				moneyshow:false,
				checki:false,
				};
		},
		onLoad() {
			this.checkuserinfo()
			this.checklist()
		},
		onShow() {
			this.checkuserinfo()
			this.checklist()
		},
		methods:{
			goto(url){
				uni.navigateTo({
					url:url
				})
			},
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
					if(!_this.userinfo.payPwd){
						_this.showmoadl('为了用户的安全，请设置支付密码')
						_this.settimeurl('/pages/PaymentPassword/PaymentPassword')
					}
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
					this.yelist = res.result
					
			},
			showmoadl(msg){
				uni.showToast({
					title: msg,
					icon:"none",
					duration: 1000
				});
			},
			settimeurl(url){
				setTimeout(()=>{
					uni.navigateTo({
						url:url
					})
				},1000)
			}
		}
	}
</script>

<style scoped lang="scss">
	@import 'MyWallet.scss';
</style>
