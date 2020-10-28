<template>
	<view class="shopList">
		<view class="shoListItem" v-for="(item,index) in content" :key="index">
		<navigator  hover-class="none"  :url="'../detail/detail?id='+item.id">
			<image :src="imgUrl+item.mainimage" class="shopImg" mode=""></image>
			<view class="shopTitle">
				{{item.smalltitle}}
			</view>
			
			<!-- //{{apiUrl}}
			{{imgUrl+item.mainimage}} -->
			<view class="shopActive">
				<view class="shopActiveLab" v-if="item.tag==1">
					新品
				</view>
				<view class="shopActiveLab" v-if="item.tag==2">
					热销
				</view>
				<view class="shopActiveLab" v-if="item.tag==3">
					促销
				</view>
				<view class="shopActiceTxt">
					{{item.summary}}
				</view>
			</view>
			<view class="shopPrice">
				¥{{item.price}}
			</view>
		</navigator>
	</view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				imgUrl:this.$imgUrl
			}
		},
		props:["content"],
		
		
		
	}
	
</script>

<style>
	.shopList{display: flex;justify-content: space-between;flex-wrap: wrap;}
	
	.shoListItem{width: 370rpx;height: 540rpx;background: #fff;overflow: hidden;
	margin-top:10rpx;}
	.shoListItem .shopImg{height: 316rpx;width: 316rpx;display: block;margin:40rpx auto 0;}
	.shoListItem .shopTitle{line-height: 50rpx;
	height: 50rpx;font-size: 28rpx;text-align: center;}
	.shoListItem .shopActive{height: 40rpx;width: 346rpx;margin:0 auto;
	border:1rpx solid #d9000b;display: flex;}
	.shopActiveLab{width: 70rpx;background: #d9000b;font-size: 24rpx;
	line-height: 40rpx;color: #fff;text-align: center;}
	.shopActiceTxt{width: 276rpx;font-size: 24rpx;color: #d9000b;
	overflow: hidden;text-overflow: ellipsis;white-space: nowrap;line-height: 40rpx;}
	.shopPrice{font-size: 28rpx;color:#d9000b ;text-align: center;line-height: 80rpx;}
</style>
