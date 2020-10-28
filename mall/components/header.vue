<template>
	<view>
		
		<view class="status"></view>
		
		<view class="header">
			
			<view class="navbar">
				<view class="search">
					<image src="../../static/image/search.png" mode=""></image>
				</view>
				<view class="logo">
					<image src="../../static/image/logo.png" mode=""></image>
				</view>
				<view class="cart">
					<image src="../../static/image/trolley.png" mode=""></image>
				</view>
			</view>
			<view class="menuTab">
				<view class="menuTabItem menuActive">
					推荐
				</view>
				<view class="menuTabItem" v-for="(item,index) in content" :key="index">
					{{item.catename}}
				</view>
				
			</view>
		</view>
		<view class="headerEmpty"></view>
	</view>

</template>

<script>
	export default{
		props:['content']
	}
</script>

<style>
	.status{
		/* height: var(--status-bar-height); */
		height: 180rpx;
		padding-top: var(--status-bar-height);
		width: 100%;
		background: #fff;
	
		
	}
	
	.header{height: 180rpx;position: fixed;width: 100%;background: #fff;z-index: 9;
	top:0;padding-top:var(--status-bar-height);}
	.navbar{height: 110rpx;display: flex;margin:0 20rpx;align-items: center;
	justify-content: space-between;}
	.navbar .search image,.navbar .cart image{width: 30rpx;height: 30rpx;}
	.navbar .logo image{width: 140rpx;height: 30rpx;}
	.menuTab{height:70rpx ;display: flex;margin:0 20rpx;justify-content: space-between;}
	.menuTab .menuTabItem{font-size: 28rpx;color: #000;width: 60rpx;height: 55rpx;line-height: 55rpx;}
	.menuTab .menuTabItem.menuActive{color: #0bbbef;border-bottom: 1rpx solid #0bbbef;}
	.headerEmpty{}
</style>
