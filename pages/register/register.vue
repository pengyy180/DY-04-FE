<template>
  <view class="page">
	  <view style="width: 100%;height: 20px;"></view>
    <view class="logo">
      <image src="../../static/alibabalogo.svg" mode=""></image>
	</view>
	<!-- <view class="logo2">
	  <image src="../../static/styleimg/masha.jpg" mode=""></image>
	</view> -->
	  <u-upload
		class="uploadimg"
	    :fileList="fileList1"
	    @afterRead="afterRead"
	    @delete="deletePic"
	    name="1"
	    multiple
	    :maxCount="1"
		width="250"
			height="250"
	  ></u-upload>
    
    <!-- 填写区 -->
    <view class="input-info">
      
      <view class="info">
        <input
          type="text"
          v-model="form.customName"
          placeholder="请输入姓名"
        />
        <view class="more"> </view>
      </view>
      <view class="info labelcss" style="color:grey;">请选择性别
       
		<radio-group  class="labelcss" @change="radioChange">
						<label  class="labelcss " style="width: 80px;" v-for="(item, index) in radiolist1" :key="item.value">

								<radio style="margin-right:10px;" :value="item.value" :checked="index === current" />

							<view>{{item.name}}</view>
						</label>
					</radio-group>
	  </view>

      <view class="info">
        <input
          type="tel"
          v-model="form.phone"
          maxlength="11"
          placeholder="请输入手机号"
        />
        <view class="more"> </view>
      </view>
 
      <view class="info">
        <input
          :password="!isPassword"
          maxlength="26"
          v-model="form.password"
          placeholder="请输入密码"
        />
        <view class="more">
          <text
            class="iconfont"
            :class="isPassword ? 'icon-eye-on' : 'icon-eye-off'"
            @click="isPassword = !isPassword"
          ></text>
        </view>
      </view>
    </view>
    <!-- 按钮 -->
    <view class="btn-info">
      <view
        class="btn"
        :style="isRegister ? 'opacity:1' : 'opacity:0.4'"
        @click="isRegister ? onRegister() : ''"
      >
        <text>注册</text>
      </view>
    </view>
    <!-- 操作 -->
    <view class="operation">
      <text></text>
      <text @click="onLogin" style="text-align: center;">已有账号?登录</text>
    </view>
  </view>
</template>

<script>
import { register } from "../../api/login";
import __config from "../../config/env.js";
export default {
  data() {
    return {
		current:0,
      isPassword: false,
      isRegister: false,
      // 表单
      form: {
        customName: "",
        customSex: 1,
        headPortrait: "",
        phone: "",
        // code: '',
        password: "",
      },

      fileList1: [],
      basePath: __config.imgaction, // 图片地址
      action: __config.action, // 图片地址

      // 基本案列数据

	   radiolist1: [{
	                      value: '1',
	                      name: '男',
	                      checked: true
	                  },
	                  {
	                      value: '2',
	                      name: '女'
	                  }],
      // u-radio-group的v-model绑定的值如果设置为某个radio的name，就会被默认选中
      radiovalue1: "男",
    };
  },
  methods: {
    onLogin() {
      uni.redirectTo({
        url: "/pages/login/login",
      });
    },
	showmoadl(msg){
		uni.showToast({
			title: msg,
			duration: 2000
		});
	},
    /**
     * 注册点击
     */
    onRegister() {
		console.log(this.form)
		if(!this.form.phone){
			this.showmoadl('请输入手机号码')
		}else if(!this.form.password){
			this.showmoadl('请输入密码')
		}else if(!this.form.customName){
			this.showmoadl('请输入名称')
		}else if(!this.form.customSex){
			this.showmoadl('请选择性别')
		}else{
			  register(this.form).then((res) => {
			    console.log(res);
					if(res.code == 200){
						uni.setStorage({
						  key: 'token',
						  data: res.result.token
						});
						uni.setStorage({
						  key: 'user',
						  data: res.result.psCustomMain
						});
						this.showmoadl('登录成功')
						uni.reLaunch({
							url:'/pages/home/home'
						})
					}
			  });
		}
		
		
      // if (!this.form.phone || !this.form.password || !this.form.customName || this.form.headPortrait || this.form.customSex) {
      //   return;
      // } else {
      //   let fff = JSON.parse(JSON.stringify(this.form));
      //   register(fff).then((res) => {
      //     console.log(res);
      //   });
      // }
    },
	 radioChange(evt){

		 this.form.customSex = evt.detail.value
	            for (let i = 0; i < this.radiolist1.length; i++) {
	                if (this.radiolist1[i].value === evt.detail.value) {
						this.form.customSex = i
	                    this.current = i;
	                    break;
	                }
	            }
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
            this.form.headPortrait = data.message;
			console.log(this.form.headPortrait);
            setTimeout(() => {
              resolve(res.data.data);
            }, 1000);
          },
        });
      });
    },
  },
  watch: {
    // && newValue.code
    form: {
      handler(newValue, oldValue) {
        if (newValue.phone && newValue.password) {
          this.isRegister = true;
        } else {
          this.isRegister = false;
        }
      },
      deep: true,
    },
  },
};
</script>

<style scoped lang="scss">
@import "register.scss";
</style>
