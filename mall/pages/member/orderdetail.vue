<template>
	<view class="">
		<view class="orderDetailTop">
			订单状态:
			<!-- 1表示待付款2表示已支付待发货，3：已发货待收货4：已收货待评价5订单完成6已取消 -->
			<text v-if="orderdetail.status==1">待付款</text>
			<text v-if="orderdetail.status==2">待发货</text>
			<text v-if="orderdetail.status==3">待收货</text>
			<text v-if="orderdetail.status==4">待评价</text>
			<text v-if="orderdetail.status==5">已完成</text>
			<text v-if="orderdetail.status==6">已取消</text>
		</view>
		<view class="orderAddress">
			<view class="addressIcon">
				<i class="iconfont">&#xe622;</i>
			</view>
			<view class="addressContent">
				<view class="address-title">
					收货人:{{orderdetail.username}}  ({{orderdetail.telphone}})
				</view>
				<view class="address-info">
					{{orderdetail.city}}{{orderdetail.address}}
				</view>
			</view>
			<view class="addressLine">
				
			</view>
		</view>
	
		<view class="orderShopList">
			<view class="shopListItem" v-for="(item,index) in orderdetail.shop" :key="index">
				<image :src="imgUrl+item.mainimage" mode=""></image>
				<view class="shopListContent">
					<view class="contentTitle">
						{{item.title}}
					</view>
					<view class="contentAttr">
						{{item.attr_0}}{{item.attr_1}}
					</view>
					<view class="contentPrice">
						¥{{item.price}}<text>×{{item.number}}</text>
					</view>
				</view>
			</view>
		</view>
		<view class="orderInfoList">
			<view class="orderInfoItem">
				应付金额<text>¥{{orderdetail.allprice-orderdetail.logisticsprice}}</text>
			</view>
			<view class="orderInfoItem">
				配送方式<text>{{orderdetail.logistics}}({{orderdetail.logisticsprice}})</text>
			</view>
			<view class="orderInfoItem messageBox">
				请留意
			</view>
			<view class="orderInfoItem">
				总计<text>共1件商品合计：¥{{orderdetail.allprice}}</text>
			</view>
			<view class="orderInfoItem">
				订单编号<text>{{orderdetail.ordernum}}</text>
			</view>
			<view class="orderInfoItem">
				下单时间<text>{{orderdetail.time}}</text>
			</view>
			<view class="orderInfoItem" v-if="orderdetail.paytype==''">
				支付方式
				<text v-if="orderdetail.paytype==1">支付宝支付</text>
				<text v-if="orderdetail.paytype==1">微信支付</text>
			</view>
			<view class="orderInfoItem" v-if="orderdetail.paytime==''">
				支付时间<text>{{orderdetail.paytime}}</text>
			</view>
			
		</view>
		<view style="height: 100rpx;">
			
		</view>
		<view class="detailBottom">
			<view class="pay">去支付</view>
			<view class="cancel">取消</view>
		</view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				orderdetail:{},
				imgUrl:this.$imgUrl
			}
		},
		onLoad(option) {
			console.log(option.orderid)
			this.getData(option.orderid)
		},
		methods:{
			getData(orderid){
				this.$request('/member/orderdetail',{orderid:orderid})
				.then(res=>{
					console.log(res.data)
					this.orderdetail=res.data
				})
			}
		}
	}
</script>

<style>
	.orderDetailTop{height: 100rpx;line-height: 100rpx;padding: 0 30rpx;
	font-size: 28rpx;border-top: 1rpx solid #e5e5e5;border-bottom: 20rpx solid #f4f4f4;}
	.orderDetailTop text{color: #00C3F5;}
	
	.orderAddress{display: flex;padding-right: 30rpx;
	position: relative;height: 200rpx;border-bottom: 20rpx solid #f4f4f4;}
	.addressIcon{width: 100rpx;display:flex;justify-content: center;
	font-size: 36rpx;color: #999;line-height: 95rpx;}
	.addressContent{flex: 1;}
	.address-title{font-size: 28rpx;margin-top:10rpx;line-height: 68rpx;}
	.address-info{font-size: 24rpx;line-height: 34rpx;color: #999;}
	.addressLine {
	    height: 4px;
	    background-image: -webkit-linear-gradient(315deg,#82c9ff 8px,transparent 0,transparent 16px,#ff8282 0,#ff8282 32px,transparent 0,transparent 0,transparent 40px,#82c9ff 0,#82c9ff);
	    background-image: linear-gradient(135deg,#82c9ff 8px,transparent 0,transparent 16px,#ff8282 0,#ff8282 32px,transparent 0,transparent 0,transparent 40px,#82c9ff 0,#82c9ff);
	    background-color: #fff;
	    background-size: 68px 4px;
	    width: 100%;
	    position: absolute;
	    bottom: 0;
	}
	.shopListItem{display: flex;margin:0 20rpx;border-top:1rpx solid #e5e5e5;}
	.orderShopList .shopListItem:nth-of-type(1){border:none}
	.shopListItem image{width: 200rpx;height: 200rpx;margin:10rpx 0;}
	.shopListContent{flex: 1;}
	.shopListContent .contentTitle{font-size: 28rpx;line-height: 40rpx;
	overflow: hidden; margin-top:10rpx;
	 text-overflow: ellipsis;
	 display: -webkit-box;
	 -webkit-line-clamp: 2; 
	 -webkit-box-orient: vertical;}
	 .shopListContent .contentAttr{font-size: 24rpx;color: #999;line-height: 40rpx;}
		 .shopListContent .contentPrice{ line-height: 80rpx;
		 color: red;font-size: 28rpx;display: flex;justify-content: space-between;}
		 .shopListContent .contentPrice text{color: #999;}
		 
		 
		 .orderInfoList{border-top: 1rpx solid #e5e5e5;
		padding: 20rpx 0;}
		 .orderInfoItem{margin:0 20rpx;font-size: 28rpx;display: flex;justify-content: space-between;
		 line-height: 80rpx;}
		 
		 .messageBox{background: #f4f4f4;border-radius: 80rpx;text-indent: 30rpx;
		 color: #999;}
		 .orderInfoItem text{color: #999;}
		 
		 .detailBottom{display: flex;justify-content: flex-end;
		 height: 100rpx;align-items: center;position: fixed;bottom:0;
		 width: 100%;background: #fff;border-top:1rpx solid #e5e5e5}
		 .detailBottom .pay{width: 150rpx;height: 50rpx;background: #00C3F5;
		 
		 color: #fff;font-size: 28rpx;text-align: center;line-height: 50rpx;}
		 .detailBottom .cancel{width: 150rpx;height: 50rpx;background: #f4f4f4;
		 color: #666;font-size: 28rpx;text-align: center;line-height: 50rpx;
		 margin:0 25rpx
		 }
</style>
