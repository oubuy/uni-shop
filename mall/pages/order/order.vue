<template>
	<view v-if="show">
		<view style="height: 20rpx;" >
			
		</view>
		<view class="address" v-if="addressFalg==1">
			<view class="addressBox" @click="listAddress">
				<view class="addressIcon">
					<i class="iconfont">&#xe622;</i>
				</view>
				<view class="addressContent" v-if="address">
					<view class="addressUser">
						收货人:{{address.username}}  {{address.telphone}}
					</view>
					<view class="addressInfo">
						{{address.city}}{{address.address}}
					</view>
				</view>
				<view class="addressMore">
					<i class="iconfont">&#xe60b;</i>
				</view>
			</view>
			<view class="addressLine">
				
			</view>
		</view>
		<view class="addressEmpty" v-else @click="addAddress">
			+暂无收货地址，请添加
		</view>
		<view class="shoplist">
			<view class="shopItem" v-for="(item,index) in shop" :key="index">
				<view class="shopItemLeft">
					<image :src="imgUrl+item.mainimage" mode=""></image>
				</view>
				<view class="shopItemRight">
					<view class="shopTitle">
						{{item.title}}
					</view>
					<view class="shopAttr">
						{{item.attr_0}},{{item.attr_1}}
					</view>
					<view class="shopPrice">
						<text>￥{{item.price}}</text><text class="num">×{{item.num}}</text>
					</view>
				</view>
			</view>
			
			
		</view>
		<view class="orederPrice">
			<view class="orderPriceItem">
				<view class="orderPriceItemTitle">应付金额</view>
				<view class="orderPriceItemPrice">¥{{total.price|toFixed}}</view>
			</view>
			<view class="orderPriceItem">
				<view class="orderPriceItemTitle">配送方式</view>
				<view class="orderPriceItemPrice">
					<picker @change="logistics" 
					range-key="name"
					:value="index" :range="array">
					    <view class="uni-input">{{array[index].name}}</view>
					</picker>
					<i class="iconfont">&#xe60b;</i>
				</view>
			</view>
			<view class="orderMessage">
				<input type="text" v-model="message" placeholder="请留言" />
			</view>
			<view class="orderPriceItem">
				<view class="orderPriceItemTitle">总价</view>
				<view class="orderPriceItemPrice">合计{{total.num}}件商品,总共¥{{endPrice|toFixed}}</view>
			</view>
		</view>
		<view style="height: 100rpx;"></view>
		
		<view class="orderBottom">
			<text class="totalPrice">总计：¥{{endPrice|toFixed}}</text>
			<view class="orderBottomBtn" @click="payBtn">立即购买</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				array:[{"name":"请选择配送方式"}],
				index:0,
				shop:[],
				total:{},
				address:{},
				addressFalg:'',
				endPrice:'',
				message:"",
				show:false,
				imgUrl:this.$imgUrl
			}
		},
		onLoad() {
			this.getData()
			
		},
		onShow() {
			this.getAddress()
		},
		filters:{
			toFixed(data,num=2){
				return parseFloat(data).toFixed(num)
			}
		},
		methods: {
			listAddress(){
				this.$href("../address/list?back=1")
			},
			addAddress(){
				this.$href('../address/add?back=1')
			},
			getAddress(){
				this.$request("/member/getOrderAddress",{
					"id":uni.getStorageSync("addressid")
				})
				.then(res=>{
					console.log(res)
					uni.setStorageSync("addressid",res.data.id)
					this.address=res.data
					this.addressFalg=res.code
				})
			},
			getData(){
				this.$request("/member/getBuyShopInfo",{
					"data":uni.getStorageSync("orderShop")
				}).then(res=>{
					console.log(res)
					this.shop=res.data.shop
					this.total=res.data.all
					this.array=this.array.concat(res.data.logistics)
					this.endPrice=res.data.all.price
					this.show=true
				})
			},
			logistics(e){
				this.index=e.target.value
				if(this.index!=0){
					this.endPrice=parseFloat(this.total.price)+parseFloat(this.array[this.index].price);
				}else{
					this.endPrice=this.total.price
				}
				
			},
			payBtn(){
				if(this.index==0){
					uni.showToast({
						title:"请选择配送方式",
						icon:"none"
					})
					return;
				}
				if(uni.getStorageSync('addressid')==''){
					uni.showToast({
						title:"请先添加收货地址",
						icon:"none"
					})
					return;
				}
				console.log(this.array[this.index].id)
				this.$request("/member/order",{
					orderShop:uni.getStorageSync('orderShop'),
					addressid:uni.getStorageSync('addressid'),
					price:this.endPrice,
					logistics:this.array[this.index].id,
					message:this.message
				}).then(res=>{
					if(res.code==1){
						this.$href('pay?id='+res.data.id)
					}
				})
				// uni.navigateTo({
				// 	url:"pay"
				// })
			}
		}
	}
</script>

<style>
	.addressEmpty{width: 695rpx;height: 85rpx;background: #fff;
	text-align: center;line-height: 85rpx;
	border-radius: 10rpx;margin:0rpx auto 0;font-size: 28rpx;color: #0cbaef;}
	.orderBottom{position: fixed;bottom: 0;z-index: 9;
	background: #fff;height: 100rpx;width: 100%;display: flex;
	justify-content: space-between;align-items: center;}
	.totalPrice{margin-left:30rpx;font-size: 28rpx;color: #000;}
	.orderBottomBtn{background: #00c2f5;width: 300rpx;height: 100rpx;
	font-size: 28rpx;color: #fff;line-height: 100rpx;text-align: center;}
	
.orederPrice{width: 695rpx;background: #fff;margin:20rpx auto;
padding:25rpx 0;height: auto;overflow: hidden;border-radius: 10rpx;
}
	.orderPriceItem{display: flex;height: 80rpx;align-items: center;
	justify-content: space-between;font-size: 28rpx;padding:0 20rpx;
}
.orderMessage{margin:0 20rpx;background: #f4f4f4;height: 70rpx;
border-radius: 70rpx;display: flex;align-items: center;}	

.orderMessage input{font-size: 28rpx;margin-left:30rpx;width: 100%;}
.orderPriceItemPrice{color: #999;display: flex;align-items: center;}
.orderPriceItemPrice i{font-size: 32rpx;line-height: 80rpx;}	
page{background: #f4f4f4;}
.address{position: relative;width: 695rpx;height: 200rpx;
margin:0 auto;border-radius: 10rpx;overflow: hidden;}
.addressBox{width: 695rpx;height: 200rpx;background: #fff;
margin:0rpx auto;display: flex;}
.addressIcon{width:100rpx;display: flex;justify-content: center;line-height:90rpx;}
.addressContent{flex:1}
.addressContent .addressUser{font-size: 28rpx;color: #000;line-height: 70rpx;
padding-top:10rpx;}
.addressContent .addressInfo{color: #999;font-size: 24rpx;}
.addressMore{width: 100rpx;display: flex;align-items: center;justify-content: center;}
.addressLine{
	    height: 8rpx;
	    background-image: -webkit-linear-gradient(315deg,#82c9ff 8px,transparent 0,transparent 16px,#ff8282 0,#ff8282 32px,transparent 0,transparent 0,transparent 40px,#82c9ff 0,#82c9ff);
	    background-image: linear-gradient(135deg,#82c9ff 8px,transparent 0,transparent 16px,#ff8282 0,#ff8282 32px,transparent 0,transparent 0,transparent 40px,#82c9ff 0,#82c9ff);
	    background-color: #fff;
	    background-size: 136rpx 8rpx;
		width: 100%;
		position: absolute;bottom: 0;
}
.shoplist{width: 695rpx;background: #fff;margin:20rpx auto 0;border-radius: 10rpx;}
.shoplist .shopItem{display: flex;padding:10rpx 0rpx;
margin:0 20rpx;
border-top:1rpx solid #e5e5e5}
.shoplist .shopItem:nth-of-type(1){border:none}
.shopItem .shopItemLeft image{width: 200rpx;height: 200rpx;}
.shopItem .shopItemRight{flex:1}
.shopItem .shopItemRight .shopTitle{font-size: 28rpx;
line-height: 40rpx;overflow: hidden; 
text-overflow: ellipsis;
display: -webkit-box;
-webkit-line-clamp: 2; 
-webkit-box-orient: vertical;
}
.shopItem .shopItemRight .shopAttr{font-size: 24rpx;color: #999;
line-height: 40rpx;}
.shopItem .shopItemRight .shopPrice{font-size: 28rpx;display: flex;
justify-content: space-between;line-height: 80rpx;color: red;}
.shopItem .shopItemRight .shopPrice .num{color: #999;}
</style>
