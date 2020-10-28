<template>
	<view>
		<view class="search">
			<view class="searchBox">
				<navigator url="#">
					<image src="../../static/image/search2.png" mode=""></image>
					搜索全部
				</navigator>
			</view>
		</view>
		<view class="cate">
			<view class="cateleft">
				<view class="cateNameList">
					<view class="cateNameItem cateActive">
						精品推荐1
						<view class="cateLine"></view>
					</view>
					<view class="cateNameItem">
						精品推荐1
						<view class="cateLine"></view>
					</view>
					<view class="cateNameItem">
						精品推荐1
						<view class="cateLine"></view>
					</view>
					<view class="cateNameItem">
						精品推荐1
						<view class="cateLine"></view>
					</view>
					<view class="cateNameItem">
						精品推荐1
						<view class="cateLine"></view>
					</view>
					<view class="cateNameItem">
						精品推荐1
						<view class="cateLine"></view>
					</view>
					<view class="cateNameItem">
						精品推荐1
						<view class="cateLine"></view>
					</view>
					<view class="cateNameItem">
						精品推荐1
						<view class="cateLine"></view>
					</view>
					<view class="cateNameItem">
						精品推荐1
						<view class="cateLine"></view>
					</view>
					<view class="cateNameItem">
						精品推荐1
						<view class="cateLine"></view>
					</view>
					<view class="cateNameItem">
						精品推荐1
						<view class="cateLine"></view>
					</view>
					<view class="cateNameItem">
						精品推荐1
						<view class="cateLine"></view>
					</view>
				</view>
			</view>
			<view class="cateright">
				<view class="cateRightScroll">
					<view class="cateItem">
						<view class="caterightTitle">
							——<text>精品推荐1</text>——
						</view>
						<view class="cateRightList">
						<view class="cateRightItem">
							<image src="../../static/image/p1.png" mode=""></image>
							<text>手机</text>
						</view>
						<view class="cateRightItem">
							<image src="../../static/image/p1.png" mode=""></image>
							<text>手机</text>
						</view>
						<view class="cateRightItem">
							<image src="../../static/image/p1.png" mode=""></image>
							<text>手机</text>
						</view>
						<view class="cateRightItem">
							<image src="../../static/image/p1.png" mode=""></image>
							<text>手机</text>
						</view>
					</view>
					</view>
					<view class="cateItem">
						<view class="caterightTitle">
							——<text>精品推荐1</text>——
						</view>
						<view class="cateRightList">
						<view class="cateRightItem">
							<image src="../../static/image/p1.png" mode=""></image>
							<text>手机</text>
						</view>
						<view class="cateRightItem">
							<image src="../../static/image/p1.png" mode=""></image>
							<text>手机</text>
						</view>
						<view class="cateRightItem">
							<image src="../../static/image/p1.png" mode=""></image>
							<text>手机</text>
						</view>
						<view class="cateRightItem">
							<image src="../../static/image/p1.png" mode=""></image>
							<text>手机</text>
						</view>
					</view>
					</view>
					<view class="cateItem">
						<view class="caterightTitle">
							——<text>精品推荐1</text>——
						</view>
						<view class="cateRightList">
						<view class="cateRightItem">
							<image src="../../static/image/p1.png" mode=""></image>
							<text>手机</text>
						</view>
						<view class="cateRightItem">
							<image src="../../static/image/p1.png" mode=""></image>
							<text>手机</text>
						</view>
						<view class="cateRightItem">
							<image src="../../static/image/p1.png" mode=""></image>
							<text>手机</text>
						</view>
						<view class="cateRightItem">
							<image src="../../static/image/p1.png" mode=""></image>
							<text>手机</text>
						</view>
					</view>
					</view>
					<view style="height: 30rpx;width: 100%;"></view>
				</view>
				
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>

<style>
	page{background: #f7f7f7;}
.search{height: 110rpx;width: 100%;background: #fff;display: flex;
justify-content: center;align-items: center;}
.searchBox{width: 690rpx;height: 70rpx;border-radius: 10rpx;
font-size: 28rpx;color: #999;background: #f7f7f7;}
.searchBox navigator{display: flex;justify-content: center;align-items: center;
height: 70rpx;}
.searchBox image{width: 38rpx;height: 38rpx;margin-right: 12rpx;}
.cate{position: absolute;top:120rpx;bottom:0;width: 100%;
display: flex;}
.cateleft{width: 200rpx;background: #f7f7f7;height: 100%;}
.cateNameList{height: 100%;overflow: auto;}
.cateRightList{height: auto;overflow: hidden;}
.cateNameItem{font-size: 28rpx;color: #000;
line-height: 100rpx;text-align: center;position:relative}
.cateActive{background: #fff;}
.cateLine{position: absolute;width: 4rpx;height: 30rpx;background: #f7f7f7;
top:35rpx;}
.cateActive .cateLine{background: #10b6e8;}

.cateright{width:550rpx ;background: #fff;height: 100%;}
.cateRightScroll{height: 100%;overflow: auto;}
.caterightTitle{line-height: 86rpx;padding-top: 16rpx;color: #999;font-size: 28rpx;
text-align: center;}
.caterightTitle text{padding:0 30rpx}
.cateRightItem{width: 33.33%;float:left;margin-top:20rpx;}
.cateRightItem image{width: 160rpx;height: 160rpx;display: block;margin:0 auto;}
.cateRightItem text{line-height: 36rpx;font-size: 24rpx;
text-align: center;display: block;width: 100%;;}
</style>
