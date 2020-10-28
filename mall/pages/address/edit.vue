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
			<template v-if="defaultAddress==1">
				<switch checked="true" color="#0bbbef"
				style="transform:scale(0.8)" @change="defaultChange"/>
			</template>
			<template v-else>
				<switch color="#0bbbef"
				style="transform:scale(0.8)" @change="defaultChange"/>
			</template>
			
		</view>
		
		
		<view class="saveAddress" @click="editAddress">
			保存收货地址
		</view>
		
		 
	</view>
</template>

<script>
	import pickerAddress from '../../components/pickerAddress/pickerAddress.vue'
	export default{
		data(){
			return{
				username:'',
				telphone:'',
				address:'',
				defaultAddress:1,
				sex:0,
				city: '请选择收货地址',
				aid:'',
				back:''
			}
		},
		components:{
		    pickerAddress
		},
		onLoad(option) {
			console.log(option.id)
			this.aid=option.id
			this.getAddress(option.id)
			this.back=option.back
		},
		methods:{
			defaultChange(e){
				if(e.target.value==true){
					this.defaultAddress=1;
				}else{
					this.defaultAddress=0;
				}
			},
			sexChange(index){
				this.sex=index
			},
			getAddress(id){
				this.$request('/member/getAddressInfo',{id:id})
				.then(res=>{
					this.username=res.data.username
					this.telphone=res.data.telphone
					this.city=res.data.city
					this.address=res.data.address
					this.defaultAddress=res.data.default
					this.sex=res.data.sex
					console.log(res.data)
				})
			},
			change(data) {
				this.city = data.data.join('')
				console.log(data.data.join(''))
			},
			editAddress(){
				//验证表单
				if(!this.check.username(this.username)){return;}
				if(!this.check.telphone(this.telphone)){return;}
				if(!this.check.city(this.city)){return}
				if(!this.check.address(this.address)){return}
				
				this.$request('/member/addressEdit',{
					username:this.username,
					telphone:this.telphone,
					city:this.city,
					address:this.address,
					default:this.defaultAddress,
					sex:this.sex,
					id:this.aid
				}).then(res=>{
					if(this.back==1){
						uni.setStorageSync("addressid",this.aid)
						this.$href("../order/order")
					}else{
						this.$href("list")
					}
					// uni.showToast({
					// 	icon:"none",
					// 	title:res.msg
					// })
				})
			}
		}
	}
</script>

<style>
	.addressItem text.sexActive{background:#0bbbef ;color: #fff;
	border:none}
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
