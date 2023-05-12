<template>
	<view class="page">
		<view class="address-input">

			<view class="list-input">
				<view class="title">
					<text>收货人</text>
				</view>
				<view class="content">
					<input type="text" v-model="userinfo.addressName" placeholder="请填写收货人姓名">
				</view>
			</view>
			<view class="list-input">
				<view class="title">
					<text>手机号码</text>
				</view>
				<view class="content">
					<input type="tel" v-model="userinfo.addressPhone" placeholder="请填写手机号码">
				</view>
			</view>
			<view class="list-input">
				<view class="title">
					<text>所在地区</text>
				</view>
				<view class="content">
				<!-- 	<picker mode="region"  @change="changresgion">
											<view class="uni-input">{{userinfo.addressProvi}}</view>
										</picker> -->
										<!-- <uni-data-picker placeholder="请选择地址" popup-title="请选择城市" collection="opendb-city-china" field="code as value, name as text" orderby="value asc" :step-searh="true" self-field="code" parent-field="parent_code"
										 @change="changresgion" @nodeclick="onnodeclick">
										    </uni-data-picker> -->
											
											
					 <lotus-address v-on:choseVal="choseValue" :lotusAddressData="lotusAddressData"></lotus-address>						
					<input type="tel" @click="openPicker" v-model="userinfo.addressProvi" placeholder="省市区县/乡镇等">
				</view>
			</view>
			<view class="list-textarea">
				<view class="title">
					<text>详细地址</text>
				</view>
				<view class="content">
					<textarea type="tel" v-model="userinfo.addressDetails" placeholder="街道/楼牌号等" />
				</view>
			</view>
		</view>
		
		
		
		<view class="tag-default">
			
			

			<view class="tag-list">
				<view class="title">
					<text>标签</text>
				</view>
				<view class="content" >
					<view v-for="(item,index) in typelist" :key="index" class="list" @click="change(index)" :class="{action:index==typeindex}">
						<text>{{item.name}}</text>
					</view>
					
				</view>
			</view>
			<!-- <view class="default-address">
				<view class="title">
					<text>默认地址</text>
				</view>
				<view class="switch-default">
					<switch class="red sm" color="#0077EE !important" checked></switch>
				</view>
			</view> -->
		</view>
		<view class="footer-btn" @click="saveadd">
			<view class="btn">
				<text>保存</text>
			</view>
		</view>
	</view>
</template>

<script>
	import { updateCustomData ,findCustomData} from "../../api/index";
	 import lotusAddress from "../../components/Winglau14-lotusAddress/Winglau14-lotusAddress.vue";
	export default {
		components:{
		        "lotus-address":lotusAddress
		    },
		data() {
			return {
				addressType: '2',
				userinfo:{},
				lotusAddressData:{
					visible:false,
					provinceName:'',
					cityName:'',
					townName:'',
				},
				region:'',
				typeindex:0,
				typelist:[
					{name:'家'},
					{name:'公司'},
					{name:'学校'},
				]

			};
		},
		onLoad(params) {
			this.addressType = params.type||'2';
			uni.setNavigationBarTitle({
				title: this.addressType === '1' ? '编辑收货地址':'新建收货地址'
			})
			this.checkuserinfo()
		},
		methods:{
			change(index){
				this.typeindex = index
			},
			changresgion(){
				
			},
			checkuserinfo(){
				let _this = this
				findCustomData().then(res=>{
					console.log(res)
					_this.userinfo = res.result
					
					
					if(_this.userinfo.addressTags == '家'){
						this.typeindex = 0
					}else if (_this.userinfo.addressTags == '公司'){
						this.typeindex = 1
					}else if (_this.userinfo.addressTags == '学校'){
						this.typeindex = 2
					}
				})
			
			},
			saveadd(){
				
				
				let reg = /^1[0-9]{10}$/
				if(!reg.test(this.userinfo.addressPhone)){

				  this.showmoadl('请输入正确手机号')
				  return;
				}else if(!this.userinfo.addressName){
					this.showmoadl('请输入收货地址')
				}else if(!this.userinfo.addressProvi){
					this.showmoadl('请选择省市区')
				}else if(!this.userinfo.addressDetails){
					this.showmoadl('请输入详细地址')
				}else{
					
			
				

						let data = 
						{
							addressName:this.userinfo.addressName,
							addressPhone:this.userinfo.addressPhone,
							addressProvi:this.userinfo.addressProvi,
							addressDetails:this.userinfo.addressDetails,
							addressTags:this.typelist[this.typeindex].name,
							id:this.userinfo.id,
						}
						
						console.log(data)
						updateCustomData(data).then((res)=>{
							if(res.code == 200){
								uni.setStorage({
								  key: 'userinfo',
								  data: res.result.psCustomMain
								});
								this.checkuserinfo()
								uni.navigateBack({
									delta:1
								})
							}
						})
					}
			},
			showmoadl(msg){
				uni.showToast({
					title: msg,
					icon:'error',
					duration: 2000
				});
			},
			//打开picker
			openPicker() {
				this.lotusAddressData.visible = true;
				// this.lotusAddressData.provinceName = '广东省';
				// this.lotusAddressData.cityName = '广州市';
				// this.lotusAddressData.townName = '白云区';
			},
			//回传已选的省市区的值
			choseValue(res){
				//res数据源包括已选省市区与省市区code
				console.log(res);
				this.lotusAddressData.visible = res.visible;//visible为显示与关闭组件标识true显示false隐藏
				//res.isChose = 1省市区已选 res.isChose = 0;未选
				if(res.isChose){
					this.lotusAddressData.provinceName = res.province;//省
					this.lotusAddressData.cityName = res.city;//市
					this.lotusAddressData.townName = res.town;//区
					this.userinfo.addressProvi = `${res.province} ${res.city} ${res.town}`; //region为已选的省市区的值
				}
			}
		}
	}
</script>

<style scoped lang="scss">
	@import 'AddressEdit.scss';
</style>
