<template>
	<view>
		<view class="member-top">
			<template v-if="!member.image">
				<image src="../../static/image/empty-tx.png" class="member-tx" mode=""></image>
			</template>
			<template v-else>
				<!-- 后续补充 -->
				<image :src="member.image" class="member-tx" mode=""></image>
			</template>
			
			
			<view class="userinfo">
				<view class="loginReg" v-if="isLogin==0">
					<navigator url="../login/login" hover-class="none">登陆/注册</navigator>
				</view>
				<view class="loginInfo" v-else>
					<template v-if="!member.nickname">
						<text class="username">英特网络</text>
					</template>
					<template v-else>
						<text class="username">{{member.nickname}}</text>
					</template>
					
					<text class="nickname">{{member.telphone}}</text>
				</view>
			</view>
			<view class="ewm">
				<image src="../../static/image/ewm.png" mode=""></image>
			</view>
			<view class="memberIcon">
				<i class="iconfont">&#xe612;</i>
				<i class="iconfont">&#xe614;</i>
			</view>
		</view>
		<view class="orderStatus">
			<view class="orderItem" @click="statusHref(1)">
				<image src="../../static/image/member-icon1.png" mode=""></image>
				<text>待付款</text>
			</view>
			<view class="orderItem" @click="statusHref(2)">
				<image src="../../static/image/member-icon2.png" mode=""></image>
				<text>待收货</text>
			</view>
			<view class="orderItem" @click="statusHref(4)">
				<image src="../../static/image/member-icon3.png" mode=""></image>
				<text>待评价</text>
			</view>
			<view class="orderItem" @click="statusHref(0)">
				<image src="../../static/image/member-icon4.png" mode=""></image>
				<text>全部订单</text>
			</view>
		</view>
		<view class="spliteLine"></view>
		<view class="menuList">
			<view class="menuItem" @click="href('../address/list')">
				<!-- <navigator url="../address/list" hover-class="none"> -->
				<image src="../../static/image/member-menu1.png" mode=""></image>
				<text>收获地址</text>
				<!-- </navigator> -->
			</view>
			<view class="menuItem">
				<image src="../../static/image/member-menu2.png" mode=""></image>
				<text>在线客服</text>
			</view>
			<view class="menuItem">
				<image src="../../static/image/member-menu3.png" mode=""></image>
				<text>邀请有礼</text>
			</view>
			<view class="menuItem">
				<image src="../../static/image/member-menu4.png" mode=""></image>
				<text>关于我们</text>
			</view>
		</view>
		<view class="logout" @click="logout" v-if="isLogin">
			退出登录
		</view>
	</view>
</template>

<script>
	import {mapState} from "vuex"
	export default {
		data() {
			return {
				code:'',
				member:[]
			};
		},
		computed:{
			...mapState(["isLogin"])
		},
		onLoad() {
			
		},
		onShow() {
			this.getData()
		},
		
		methods:{
			statusHref(index){
				this.$href('/pages/member/orderlist?status='+index)
			},
			href(url){
				this.$href(url)
			},
			logout(){
				this.$store.commit('logout')
			},
			getData(){
				this.$request('/member/index')
				.then(res=>{
					console.log(res)
					
					
					// this.code=res.code;
					
					if(res.code==1){
						this.$store.commit('login')
						this.member=res.data;
					}
				})
			}
		}
	}
</script>

<style>
.member-top{height: 336rpx;background: url(../../static/image/member-top.png) no-repeat;
background-size: 100% 336rpx;position: relative;}
.member-tx{width: 110rpx;height: 110rpx;position: absolute;top:165rpx;left:30rpx;
border-radius: 50%;}
.loginReg{width: 160rpx;height: 45rpx;color: #fff;border:1rpx solid #fff;font-size: 28rpx;
text-align: center;line-height: 45rpx;position: absolute;top:190rpx;left:160rpx;}
.loginInfo{position: absolute;top:180rpx;left:160rpx;}
.loginInfo text.username{line-height: 42rpx;height: 42rpx;font-size: 28rpx;color: #fff;display: block;}
.loginInfo text.nickname{line-height: 38rpx;height: 38rpx;font-size: 24rpx;color: #fff;display: block;}
.ewm{height: 35rpx;width: 35rpx;position: absolute;top:200rpx;right:30rpx;}
.ewm image{width: 35rpx;height: 35rpx;}
.memberIcon{position: absolute;top:50rpx;right: 30rpx;color: #fff;}
.memberIcon i{margin-left: 35rpx;}

.orderStatus{display: flex;}
.orderItem{width:25%;}
.orderItem image{width: 70rpx;height: 56rpx;display: block;margin:20rpx auto 0;}
.orderItem text{display: block;line-height: 80rpx;font-size: 28rpx;text-align: center;padding-bottom: 10rpx;}
.spliteLine{height: 20rpx;width: 100%;background: #f5f5f5;}

.menuItem {height: 90rpx;margin:0 30rpx;border-top: 1rpx solid #e5e5e5;
font-size:28rpx ;display: flex;align-items: center;
background: url(../../static/image/more.png)  no-repeat ;
background-size: 26rpx 26rpx;
background-position: right center;
}
.menuItem:nth-of-type(1){border-top:none;}
.menuItem image{width: 30rpx;height: 30rpx;margin-right: 25rpx;}
.logout{margin:30rpx;height: 85rpx;border:1rpx solid #e5e5e5;font-size: 28rpx;text-align: center;
line-height: 85rpx;}
</style>
