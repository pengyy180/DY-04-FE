<template>
  <view class="page">
		<homenav></homenav>
	    
      <view class="main" >
        <!-- banner -->
		<view class="procat">
			<view class="protitle">
				{{typename}}
			</view>
			<view class="list" >
				<view class="li3" v-for="(item, index) in goodsList2" @click="godetails(item.id)">
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
		<view class="limitpage" >
			<view class="icon" @click="uppage()">
				←
			</view>
			<view class="pages">
				Page {{pageNo}} of {{pagetotal}}
			</view>
			<view class="icon" @click="nextpage()">
				→
			</view>
		</view>
		<view class="homejieshao">
			<view class="li">
				<image class="mubiao" src="/static/hometxt.png" mode=""></image>
			</view>
			<view class="li">
				<image class="jimg1" src="/static/j.png" mode=""></image>
				<text>无转基因</text>
			</view>
			<view class="li">
				<image class="jimg2" src="/static/j1.png" mode=""></image>
				<text>客户至上</text>
			</view>
			<view class="li">
				<image class="jimg2" src="/static/j2.png" mode=""></image>
				<text>世界领先系列</text>
			</view>
			<view class="li">
				<image class="jimg2" src="/static/j3.png" mode=""></image>
				<text>可持续性</text>
			</view>
			<view class="li">
				<image class="jimg2" src="/static/j4.png" mode=""></image>
				<text>国际配送</text>
			</view>
		</view>
		<view class="aboutme">
			<view class="li">
				<image class="aboutimg1" src="/static/a1.webp" mode=""></image>
			</view>
			<view class="li">
				<image class="aboutimg1" src="/static/a2.webp" mode=""></image>
			</view>
			<view class="li">
				<image class="aboutimg2" src="/static/a3.webp" mode=""></image>
				<image class="aboutimg2" src="/static/a4.webp" mode=""></image>
			</view>
			<view class="li">
				<image class="aboutimg3" src="/static/a5.webp" mode=""></image>
				<image class="aboutimg4" src="/static/a6.webp" mode=""></image>
			</view>
		</view>
		<view class="copyright">
			© 2022, Naturecan Ltd - Bank Chambers, Stockport, SK1 1AR, UK
		</view>
		<view class="china">
			🇨🇳 CHINA
		</view>
		<view class="smallabout">
			This product is not for use by or sale to persons under the age of 18. This product should be used only as directed on the label. It should not be used if you are pregnant or nursing. Consult with a physician before use if you have a serious medical condition or use prescription medications. A Doctor's advice should be sought before using this and any supplemental dietary product. All trademarks and copyrights are property of their respective owners and are not affiliated with nor do they endorse this product. These statements have not been evaluated by the FDA. This product is not intended to diagnose, treat, cure or prevent any disease. Individual weight loss results will vary. By using this site, you agree to follow the Privacy Policy and all Terms & Conditions printed on this site. Void Where Prohibited by Law.
		</view>
       
		<!-- 今日上新 -->
        <!-- <view class="new-product">
          <view class="product-title">
            <view class="title">
              <image src="/static/hr_ico.png"></image>
              <text>今日上新1元抢购</text>
            </view>
            <view class="describe">
              <text class="cuIcon-roundright"></text>
            </view>
          </view>
          <view class="goods-list"  
		  >
            <view class="list" v-for="(item, index) in goodsList"
              @click="godetails(item.id)"
              :key="index" >
              <view class="pictrue">
                <image :src="item.productImgs"></image>
              </view>
			 <view class="price-title">
			  	{{item.productTitle}}
			  </view>	
			  <view class="price-start">
			  	<u-rate :value="item.startnumber" active-color="#2f6669" inactive-color="#b2b2b2" :allowHalf="false"></u-rate> {{item.startnumber}}
			  </view>
              <view class="price" @click="godetails(item.id)">
				  <text class="original-price">特价</text>
                <text class="selling-price">￥{{item.productPrice}}</text>

              </view>
            </view>
            
          </view>
        </view> -->
		<!-- 为你推荐 -->
        <!-- <view class="recommend-info">
          <view class="recommend-title">
            <view class="title">
              <image src="/static/wntj_title.png" mode=""></image>
            </view>
          </view>
          <view class="goods-list">
            <view
              class="list"
              v-for="(item, index) in goodsList2"
              @click="godetails(item.id)"
              :key="index"
            >
              <view class="pictrue">
                <image
                  :src="item.productImgs"
                  mode="heightFix"
                ></image>
              </view>
              <view class="title-tag">
                <view class="tag">
                  <text v-if="item.productPrice == 1">特价</text>
                  {{ item.productTitle }}
                </view>
              </view>
			 <view class="price-start">
			 	<u-rate :value="item.startnumber" active-color="#2f6669" inactive-color="#b2b2b2" :allowHalf="false"></u-rate> {{item.startnumber}}
			 </view>	
              <view class="price-info">
                <view class="user-price">
                  <text class="min">￥</text>
                  <text class="max">{{ item.productPrice }}</text>
                </view>
                <view class="vip-price">

                </view>
              </view>
            </view>
          </view>
		  <uni-load-more :status="status"></uni-load-more>

		  
        </view> -->
		
      </view>
	  
    <!-- </mescroll-body> -->
	
	
	
    <!-- <ClassifyData v-show="classifyShow != 0" ></ClassifyData> -->
    <!-- tabbar -->
	<!-- <checkdata></checkdata> -->
    <!-- <TabBar :tabBarShow="0"></TabBar> -->
  </view>
</template>

<script>
import TabBar from "../../components/TabBar/TabBar.vue";
import ClassifyData from "../../components/ClassifyData/ClassifyData.vue";
// 引入mescroll-mixins.js
// import MescrollMixin from "@/components/mescroll-uni/mescroll-mixins.js";
import { findProductList,findProductCat ,findBannerList,lastAcitiy,findKefu} from "../../api/index";
	import homenav from "../../components/homenav.vue";//判断用户信息
import __config from "../../config/env.js";
export default {
  // mixins: [MescrollMixin], // 使用mixin
  components: {
    TabBar,
    ClassifyData,
	homenav	
  },
  data() {
    return {
	  navshow:false,
	  HalfValue:5,
      action: __config.imgaction, // 图片显示地址
      imgaction: __config.oldurl, // 图片显示地址
      mescroll: null, // mescroll实例对象 (此行可删,mixins已默认)
      // 下拉刷新的配置(可选, 绝大部分情况无需配置)
      downOption: {},
      // 上拉加载的配置(可选, 绝大部分情况无需配置)
      classList: [
        {
          id: 0,
          name: "首页",
        }
        
      ],
      goodsList: [],
      goodsList2: [],
      classifyShow: 0,
      // 页面高度
      pageHeight: 500,
	  lasttime:'',//活动开始时
	  downtime:'',//倒计时
	  typeid:0,
	  timeData:{},
	  nowTime: [], // 当前时间
	  status: 'more',
	  pagesize:2,
	  pageNo:1,
	  pagetotal:0,
	  typename:'',
	  typeid:''
    };
  },
  
  
  onLoad(option) {
	
	this.typename = option.typename
	this.typeid = option.typeid
    this.funFindProductList(this.typeid,this.pageNo,this.pagesize);
	this.checkprotypelist()

  },
  onShow() {
  	
  },

  onPageScroll(e) {
    let scrollTop = e.scrollTop;
    if (scrollTop > 0) {
      this.pageHeight = 210;
    } else {
      this.pageHeight = 500;
    }
  },
  onReachBottom() {
    console.log(12333);
  },
  mounted(){
      this.$nextTick(() => { // 动态时间展示
      	setInterval(this.showTime, 1000);
		setInterval(this.checktime, 60000);
		
      });
    },
  methods: {
	  uppage(){
		  if(this.pageNo > 1){
			  this.pageNo -=1
			  this.funFindProductList(this.typeid,this.pageNo,this.pagesize);
			  uni.pageScrollTo({
			  	scrollTop: 0,
			  	duration: 1000
			  });
		  }
	  },
	  nextpage(){
		  if(this.pageNo <= this.pagesize ){
			  this.pageNo +=1
			  this.funFindProductList(this.typeid,this.pageNo,this.pagesize);
			  uni.pageScrollTo({
			  	scrollTop: 0,
			  	duration: 1000
			  });
		  }
	  },
	  //前往商品分类
	  gotype(item){
	  
	  		 uni.navigateTo({
	  		 	url:'./hometype?typeid='+item.id+'&typename='+item.name
	  		 }) 
	  },
	  gohome(){
		 uni.reLaunch({
		 	url:'/pages/home/home'
		 }) 
	  },
	
	  //客服
	  kefu() {
			findKefu().then((res)=>{
				console.log(res.result)
				window.location.href="https://chatlink.mstatik.com/widget/standalone.html?eid="+res.result
			})
		  
	  },
	  
	  async checkprotypelist(){
		  let _this = this
		  //商品分类
		  findProductCat().then((res)=>{
			  let data = []
			   res.result.forEach(function (item, index) {
				   let li = {}
				           li.id = item.id
							li.name = item.productCatName
							li.coverImg = _this.imgaction+'/'+item.coverImg
							data.push(li)
			   });
			   this.classList = data
		  })
		 
	  },
	  //商品详情页跳转
	  godetails(id){
		  uni.navigateTo({
		  	url:'/pages/GoodsDetails/GoodsDetails?id='+id
		  })
	  },
	  //下拉加载
	  onReachBottom() {
	 //  	this.pageNo +=1
		// this.funFindProductList(this.typeid);
	 //  	uni.$u.toast("上拉触发",this.pageNo)
	  	//这是上拉触发的函数.一般在这里进行分页操作. 
	      /*会有一种情况就是当数据已经全部加载完成了,再次触底还会调用接口,
	        这时就需要去记录一下是否已经全部加载完毕,加载完毕就不去请求接口了.
	        这时最常见的节流场景*/
	  },
    //   商品列表
    async funFindProductList(typeid,pageNo,pagesize) {
		console.log(typeid)
			
		const  res2  = await findProductList({catId:typeid,pageNo:pageNo,pageSize:pagesize})
			
			
			
			for (let i = 0; i < res2.result.records.length; i++) {
				
				let imglist = res2.result.records[i].productImgs.split(",")
				res2.result.records[i].productImgs = this.action + '/' +imglist[0]
				res2.result.records[i].startnumber = (Math.random()*(5-4.5)+4.5).toFixed(1)
			}
			// this.pageNo = res2.result.current
			this.pagetotal = res2.result.pages
			
			this.goodsList2 = res2.result.records

		
		
		
     
    },

    openlan() {
      this.$refs.popup.open("center");
    },
    close() {
      location.reload();

      this.$refs.popup.close();
    },
    onLocaleChange(e) {
      console.log(e);
      if (this.isAndroid) {
        uni.showModal({
          content: this.$t("index.language-change-confirm"),
          success: (res) => {
            if (res.confirm) {
              uni.setLocale(e.code);
            }
          },
        });
      } else {
        uni.setLocale(e.code);
        this.$i18n.locale = e.code;
      }
    },
    /*下拉刷新的回调, 有三种处理方式:*/
    downCallback() {
      // this.mescroll.endSuccess();
    },
    /*上拉加载的回调*/
    upCallback(page) {
      setTimeout(() => {
        this.mescroll.endByPage(10, 20);
      }, 2000);
    },
   
    /**
     * 搜索点击
     */
    onSearch() {
      uni.navigateTo({ url: "/pages/search/search" });
    },
    
    /**
     * 分类点击
     * @param {Object} item
     * @param {Number} index
     */
    async onClassify(item, index) {
      this.classifyShow = index;
	  this.goodsList = []
	  this.goodsList2 = []
	  this.typeid = item.id
	  this.status = 'more'
	  
	  this.pageNo = 1
	  this.pagesize = 10
	  await this.funFindProductList(item.id);
	  
	  console.log(item)

	  	
	  uni.pageScrollTo({
	  	scrollTop: 0,
	  	duration: 1000
	  });
	  
    },
	//秒转换时分秒
	getTimem(time) {
	            // 转换为式分秒
	            let h = parseInt(time / 60 / 60 % 24)
	            h = h < 10 ?  h : h
	            let m = parseInt(time / 60 % 60)
	             m = m < 10 ?  m : m
	            let s = parseInt(time % 60)
	             s = s < 10 ?  s : s
	            // 作为返回值返回
	            return [h, m, s]
	},
	showTime() { // 动态展示现在时间
				
		   var date = new Date();
		   var year = date.getFullYear();
		   var month = date.getMonth() + 1;
		   month = month < 10 ? "0" + month : month;
		   var day = date.getDate();
		   day = day < 10 ? "0" + day : day;
		   var week = "日一二三四五六".charAt(date.getDay()); // 使用charAt函数提取相应汉字
		   var hour = date.getHours();
		   hour = hour < 10 ? "0" + hour : hour; // 用三目运算符调整数字显示格式
		   var minute = date.getMinutes();
		   minute = minute < 10 ? "0" + minute : minute;
		   var second = date.getSeconds();
		   second = second < 10 ? "0" + second : second;
		   // 加载现在时间
		   var current =  hour + ":" + minute + ":" + second;
			   	this.nowTime = [hour,minute,second];
			   
		 },
    /**
     * 跳转点击
     * @param {String} type 跳转类型
     */
    onSkip(type) {
      switch (type) {
	  case "my":
		uni.navigateTo({
		  url: "/pages/my/my",
		});
		break;
        case "mess":
          uni.navigateTo({
            url: "/pages/Message/Message",
          });
          break;
        case "paycode":
          uni.navigateTo({
            url: "/pages/PaymentCode/PaymentCode",
          });
          break;
        case "menu":
          uni.navigateTo({
            url: "/pages/SearchGoodsList/SearchGoodsList",
          });
          break;
        case "inform":
          break;
        case "flash":
          uni.navigateTo({
            url: "/pages/FlashSale/FlashSale",
          });
          break;
        case "GoodChoice":
          uni.navigateTo({
            url: "/pages/GoodChoice/GoodChoice",
          });
          break;
        case "goods":
          uni.navigateTo({
            url: "/pages/GoodsDetails/GoodsDetails",
            animationType: "zoom-fade-out",
            animationDuration: 200,
          });
          break;
      }
    },
  },
};
</script>

<style scoped lang="scss">
@import "home.scss";
</style>
