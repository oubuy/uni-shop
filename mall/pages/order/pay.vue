<template>
	<view class="pay">
		<view class="payorder">
			<view class="payNumber">
				订单号:{{payinfo.ordernum}}
			</view>
			<view class="payPrice">
				<text>¥</text>{{payinfo.allprice}}
			</view>
			<view class="payTime">
				最迟支付时间:{{payinfo.end_time}}
			</view>
		</view>
		<view class="payType">
			<view class="payTypeTitle">
				请选择支付方式
			</view>
				<radio-group @change="radioChange">
					
				
				<view class="payTypeItem">
					<view class="payTypeItemLeft">
						<i class="iconfont" style="color: #0f9ddf;">&#xe647;</i>支付宝支付
					</view>
					<radio color="#01c2fa" value="1" checked=""></radio>
				</view>
				<view class="payTypeItem">
					<view class="payTypeItemLeft">
						<i class="iconfont" style="color: #07d21c;">&#xe63d;</i>微信支付
					</view>
					<radio color="#01c2fa" value="2"></radio>
				</view>
			</radio-group>
		
		</view>
		<view class="payButton" @click="payButton">
			确定支付
		</view>
	</view>
	
</template>

<script>
	export default{
		data(){
			return{
				type:1,
				payinfo:{},
				orderid:''
			}
		},
		onLoad(option) {
			this.orderid=option.id
			this.$request('/member/payinfo',{id:option.id})
			.then(res=>{
				this.payinfo=res.data
			})
			
			uni.getProvider({
			    service: 'payment',
			    success: function (res) {
			        console.log(res.provider)
			    }
			});
			
		},
		methods:{
			radioChange(e){
				this.type=e.target.value;
			},
			async payment(paytype){
				var orderInfo=await this.getOrderInfo(paytype);
				uni.requestPayment({
				    provider: paytype,
				    orderInfo: orderInfo.data, //微信、支付宝订单数据
				    success: (e) => {
				        console.log("success", e);
				        uni.navigateTo({
				        	url:'status'
				        })
				    },
				    fail: function (err) {
				        console.log('fail:' + JSON.stringify(err));
						
					}
				});
			},
			 payButton(){

				
				if(this.type==1){
					
					this.payment('alipay')
					
				}
				//微信支付
				if(this.type==2){
					this.payment('wxpay')
				}
			},
			getOrderInfo(type){
				
				return new Promise((reslove)=>{
					// uni.request({
					// 	url:"https://demo.dcloud.net.cn/payment/?payid="+type+"&total=0.1",
					// 	// url:'http://www.mall.com/api/pay/alipay?orderid'+this.orderid,
					// 	data:{
					// 		orderid:this.orderid
					// 	},
					// 	success(res) {
					// 		reslove(res)
					// 	}
					// })
					console.log(type)
					this.$request('/pay/'+type,{
						orderid:this.orderid
					}).then(res=>{
						reslove(res)
					})

				})
				
			}
		}
	}
</script>

<style>
	.payButton{height: 80rpx;width: 624rpx;position: fixed;
	bottom:120rpx;background: #0bbaef;left:50%;margin-left:-312rpx;
	text-align: center;line-height: 80rpx;color: #fff;font-size: 28rpx;
	border-radius: 80rpx;}
	
	
	.payType{width: 650rpx;margin:58rpx auto 0;}
	.payType .payTypeTitle{font-size: 28rpx;line-height: 55rpx;}
	.payType .payTypeItem{display: flex;justify-content: space-between;
	height: 76rpx;align-items: center;}
	.payTypeItemLeft{font-size: 28rpx;display: flex;align-items: center;}
	
	.payType .payTypeItem i{font-size: 32rpx;margin-right: 20rpx;display: block;}
	.payType .payTypeItem radio{transform: scale(0.7);}
	/* switch */
	
	
	.payNumber{font-size: 24rpx;color: #999;text-align: center;padding-top: 54rpx;}
	.payPrice{font-size: 70rpx;color: #000;height:120rpx;display: flex;align-items: center;
	justify-content: center;}
	.payPrice text{font-size: 28rpx;}
	.payTime{font-size: 24rpx;color: #999;line-height: 56rpx;text-align: center;}
</style>
