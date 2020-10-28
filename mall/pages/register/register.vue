
<template>
	<view>
		<image src="../../static/logo.png" class="logo" mode=""></image>
		<view class="formlist">
			<view class="formItem">
				<input type="text" v-model="telphone"
				 placeholder="请输入手机号" />
			</view>
			<view class="formItem">
				<input type="text" 
				 placeholder="请输入验证码" v-model="code" />
				<view class="getCode "
				:class="{activeCode:codeFlag}"
				@click="getCode"
				>{{codeTxt}}</view>
			</view>
			<view class="formItem">
				<input type="text" v-model="password" 
				 placeholder="请输入密码" />
			</view>
			<view class="registerBtn" @click="register">
				注册
			</view>
			<view class="loginTxt">
				已有账号?<navigator url="../login/login">点击登录</navigator>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				codeTxt:"获取验证码",
				codeFlag:true,
				telphone:'',
				password:'',
				code:''
			}
		},
		methods: {
			getCode(){
				if(!this.check.telphone(this.telphone)){return;}
				
				
				if(this.codeFlag==false){
					return;
				}
				
				this.sendMessage()
				
				
			},
			sendMessage(){
				uni.request({
					url:this.apiUrl+"/index/getSmsCode",
					method:"POST",
					data:{
						telphone:this.telphone
					},
					success: (res) => {
						if(res.data.result!=0){
							uni.showToast({
								title:res.data.errmsg,
								icon:'none'
							})
						}else{
							this.codeFlag=false;
							var time=10
							this.codeTxt="重新获取"+time;
							var timer=setInterval(()=>{
								if(time==1){
									this.codeTxt="获取验证码"
									this.codeFlag=true
									clearInterval(timer)
								}else{
									time--
									this.codeTxt="重新获取"+time;
								}
								
							},1000)
						}
					}
				})
			},
			register(){
				if(!this.check.telphone(this.telphone)){return;}
				if(!this.check.password(this.password)){return;}
				if(!this.check.code(this.code)){return;}
				uni.request({
					url:this.apiUrl+"/index/register",
					method:"POST",
					data:{
						telphone:this.telphone,
						password:this.password,
						code:this.code
					},
					success: (res) => {
						console.log(res)
					}
				})
			}
		}
	}
</script>

<style>
.logo{width: 250rpx;height: 250rpx;display: block;margin:70rpx auto 15rpx;}
.formItem{height: 74rpx;padding-top:36rpx;border-bottom: 1rpx solid #e5e5e5;
margin:0 100rpx;display: flex;align-items: center;
justify-content: space-between;}
.formItem input{font-size: 28rpx;}
.getCode{height: 60rpx;width: 180rpx;background: #f7f7f7;
color: #666;
line-height: 60rpx;font-size: 24rpx;border:none;text-align: center;}
.getCode.activeCode{background: #23baef;color: #fff;}
.registerBtn{margin:60rpx 100rpx 0;height: 86rpx;font-size: 28rpx;
background: #23baef;color: #fff;text-align: center;line-height: 86rpx;
}
.loginTxt{display: flex;height: 120rpx;justify-content: center;
align-items: center;font-size: 24rpx;}
.loginTxt navigator{color: #23baef;margin-left:20rpx;}
</style>
