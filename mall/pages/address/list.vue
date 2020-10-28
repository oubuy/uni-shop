<template>
	<view class="" v-if="show">
		<view class="addresslist" v-if="addressList&&addressList.length>0">
			
			<uni-swipe-action>
			    <uni-swipe-action-item 
				:options="options" @click="onClick($event,index,item.id)" @change="change"
				v-for="(item,index) in addressList" :key="index"
				>
			        <view class="addressItem" @click="addressCheck(item.id)">
			        	<view class="addressMain">
							
			        		<text class="username">{{item.username}}</text>
			        		<text class="telphone">{{item.telphone}}</text>
			        		<text class="default" v-if="item.default==1">默认</text>
			        	</view>
			        	<view class="addressInfo">
			        		{{item.city}}{{item.address}}
			        	</view>
			        </view>
			    </uni-swipe-action-item>
				
				
			</uni-swipe-action>
		
		</view>
		<view class="empty" v-else>
			<image src="../../static/default/address.png" mode=""></image>
			<text>用户没有收货地址,请添加</text>
		</view>
		
		
		<view style="height: 90rpx;">
			
		</view>
		
		<view class="addBtn" @click="addUrl" >
			添加新地址
		</view>
		
	</view>
</template>

<script>
	import uniSwipeAction from '@/components/uni-swipe-action/uni-swipe-action.vue'
	import uniSwipeActionItem from '@/components/uni-swipe-action-item/uni-swipe-action-item.vue'
	export default {
	    components: {
	        uniSwipeAction,
	        uniSwipeActionItem
	    },
		data(){
			return{
				show:false,
				options:[
				        {
				            text: '删除',
				            style: {
				                backgroundColor: '#ff0309'
				            }
				        }, {
				            text: '编辑',
				            style: {
				                backgroundColor: '#0bbbef'
				            }
				        }
				      ],
				addressList:[],
				back:''
			}
		},
		onLoad(option) {
			this.getData()
			if(option.back==1){
				this.back=option.back
			}else{
				this.back=0
			}
			
		},
		 methods:{
			 addressCheck(id){
				 if(this.back==1){
					 uni.setStorageSync("addressid",id)
					 this.$href("../order/order")
				 }
				
			 },
			 getData(){
				 this.$request("/member/addressList")
				 .then(res=>{
					 this.addressList=res.data;
					 console.log(res.data)
					 this.show=true
				 })
			 },
		    onClick(e,index,id){
				if(e.content.text=="删除"){
					console.log(index,e.content.text)
					uni.showModal({
						title: '提示',
						content: '是否要删除对应数据',
						success:res=> {
							if (res.confirm) {
								//前端删除操作
								this.addressList.splice(index,1)
								//后端删除
								this.$request('/member/addressDel',
									{
									 id:id
									}
								).then(res=>{
									console.log(res)
								})
								
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					})
				}
				if(e.content.text=="编辑"){
					console.log(index,'您点击的是编辑')
					uni.navigateTo({
						url:"edit?id="+id+"&back="+this.back
					})
				}
				
				

		    },
		    change(open){
		      console.log('当前开启状态：'+ open)
		    },
			addUrl(){
				this.$href("add?back="+this.back)
				
			}
		  }
	}
</script>

<style>
.empty image{width: 330rpx;height: 210rpx;display: block;margin: 200rpx auto 0;}
.empty text{font-size: 24rpx;color: #999;line-height: 80rpx;text-align: center;
display: block;}
	.addressItem{padding:0 35rpx;width: 100%;
	border-bottom: 1rpx solid #e5e5e5;}
	.addressMain{line-height: 40rpx;font-size: 28rpx;padding-top: 30rpx;}
	.addressMain .telphone{padding: 0 20rpx 0 10rpx;}
	.addressMain .default{background: #1fc8f2;color: #fff;font-size: 20rpx
	;padding:0 10rpx;}
	.addressInfo{font-size: 24rpx;line-height: 36rpx;
	color: #999;padding-bottom: 36rpx;}
	
	.addBtn{height: 90rpx;position: fixed;bottom: 0;width: 100%;left:0;
	background: #0bbbef;color: #fff;text-align: center;line-height: 90rpx;
	font-size: 28rpx;}
</style>
