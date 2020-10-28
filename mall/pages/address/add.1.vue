<template>
	<view class="">
		<view class="username">
			<view class="itemTitle">
				收货人：
			</view>
			<input type="text" value="" placeholder="收货人姓名" />
		</view>
		<view class="addressItem">
			<view class="itemTitle">
				
			</view>
			<text>先生</text>
			<text>女士</text>
		</view>
		<view class="addressItem">
			<view class="itemTitle">
				电话号码：
			</view>
			<input type="text" value="" placeholder="收货人的联系电话" />
		</view>
		<view class="addressItem">
			<view class="itemTitle">
				收货地址：
			</view>
			<pickerAddress class="city" @change="change">{{city}}</pickerAddress>
		</view>
		<view class="addressBox">
			<view class="itemTitle">
				详细地址：
			</view>
			<textarea value="" placeholder="请输入详细地址" />
		</view>
		<view class="defaultItem">
			<view class="itemTitle">
				默认地址：
			</view>
			<switch checked="true" color="#0bbbef" style="transform:scale(0.8)"/>
		</view>
		
		
		<view class="saveAddress">
			保存收货地址
		</view>
		
		 
	</view>
</template>

<script>
	import pickerAddress from '../../components/pickerAddress/pickerAddress.vue'
	export default{
		data(){
			return{
				 city: '请选择收货地址',
				
			}
		},
		components:{
		    pickerAddress
		},
		methods:{
			change(data) {
				this.city = data.data.join('')
				console.log(data.data.join(''))
			}
		}
	}
</script>

<style>
	.username{padding:0 30rpx;display: flex;height: 90rpx;align-items: center;}
	.itemTitle{width: 140rpx;font-size: 28rpx;}
	.username input{border-bottom: 1rpx solid #e5e5e5;flex:1;line-height: 90rpx;
	height: 90rpx ;}
	.addressItem{display: flex;
	margin: 0 30rpx;height: 90rpx;align-items: center;
	border-bottom: 1rpx solid #e5e5e5;}
	.addressItem text{width: 80rpx;height: 45rpx;display: block;
	border:1rpx solid #e5e5e5;font-size: 24rpx;margin-right: 10rpx;
	text-align: center;line-height: 45rpx;color: #999;}
	.addressBox{height: 180rpx;display: flex;margin:0 30rpx;
	border-bottom: 1rpx solid #e5e5e5;}
	.addressBox .itemTitle{width: 140rpx;line-height: 90rpx;}
	.addressBox textarea{flex: 1;height: 180rpx;padding-top: 25rpx;}
	.saveAddress{width: 600rpx;height: 80rpx;margin:80rpx auto;
	background: #0bbbef;color: #fff;font-size: 28rpx;
	text-align: center;line-height: 80rpx;border-radius: 80rpx;}
	.city{font-size: 28rpx;color: #000;}
	.defaultItem{margin: 0 30rpx;display: flex;height: 90rpx;
	align-items: center;justify-content: space-between;}
</style>
