<template>
	<view class="">
		<image src="../../static/image/success.png" mode="" class="statusImg"></image>
		<!-- <image  src="../../static/image/error.png" mode="" class="statusImg"></image> -->
		<view class="payStatus">支付成功</view>
		<view class="payTip">支付成功后商家会尽快发货</view>
		<view class="payTip">请小主人耐心等待哟~</view>
		<view class="statusBtn">
			<view class="statausOrder">
				查看订单
			</view>
			<view class="statusIndex">
				回到首页
			</view>
		</view>
	</view>
</template>

<script>
	
</script>

<style>
	.statusImg{width: 160rpx;height: 160rpx;display: block;margin:120rpx auto 16rpx;}

	.payStatus{font-size: 32rpx;text-align: center;line-height: 90rpx;}
	.payTip{font-size: 28rpx;color: #999;line-height: 50rpx;text-align: center;}
	.statusBtn{margin-top: 80rpx;display: flex;justify-content: center;}
	.statusIndex{width: 200rpx;height: 50rpx;background: #0bbaef;margin:0 38rpx;
	color: #fff;font-size: 28rpx;text-align: center;line-height: 50rpx;}
	.statausOrder{width: 200rpx;height: 50rpx;border:1rpx solid #e5e5e5;font-size: 28rpx;text-align: center;line-height: 50rpx;
	color: #999;margin:0 38rpx;}
</style>
