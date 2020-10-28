<template>
	<view class="">
		<view class="username">
			<view class="itemTitle">
				收货人：
			</view>
			<input type="text" v-model="username" placeholder="收货人姓名" />
		</view>
		<view class="addressItem">
			<view class="itemTitle">
				
			</view>
			<text :class="{sexActive:sex==0}" @click="sexChange(0)">先生</text>
			<text :class="{sexActive:sex==1}" @click="sexChange(1)">女士</text>
		</view>
		<view class="addressItem">
			<view class="itemTitle">
				电话号码：
			</view>
			<input type="text" v-model="telphone" placeholder="收货人的联系电话" />
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
			<textarea v-model="address" placeholder="请输入详细地址" />
		</view>
		<view class="defaultItem">
			<view class="itemTitle">
				默认地址：
			</view>
			<switch @change="defaultChange" checked="true" color="#0bbbef" style="transform:scale(0.8)"/>
		</view>
		
		
		<view class="saveAddress" @click="addAddress">
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
				username:"",
				telphone:'',
				address:'',
				default:1,
				sex:0,
				back:''
			}
		},
		components:{
		    pickerAddress
		},
		onLoad(option) {
			this.back=option.back
		},
		methods:{
			sexChange(index){
				this.sex=index
			},
			defaultChange(e){
				if(e.target.value==true){
					this.default=1
				}else{
					this.default=0
				}
			},
			change(data) {
				this.city = data.data.join('')
				console.log(data.data.join(''))
			},
			addAddress(){
				//验证表单
				if(!this.check.username(this.username)){return;}
				if(!this.check.telphone(this.telphone)){return;}
				if(!this.check.city(this.city)){return}
				if(!this.check.address(this.address)){return}
				
				
				this.$request('/member/addressAdd',{
					username:this.username,
					telphone:this.telphone,
					city:this.city,
					address:this.address,
					default:this.default,//0表示不默认1表示默认
					sex:this.sex//0表示先生，1表示女士
				})
				.then(res=>{
					//需要跳转到order页面
					if(this.back==1){
						this.$href("../order/order")
					}else{
						this.$href("list")
					}
					
				})
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
	
	.addressItem text.sexActive{background:#0bbbef ;color: #fff;
	border:none}
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
