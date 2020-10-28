<template>
	<view v-if="show">
		<view class="cartEmpty" v-if="isLogin==0">
			<image src="../../static/image/cartEmpty.png" mode="" class="cartEmptyImg"></image>
			<view class="cartEmptyTip">
				亲,您需要登录才能查看购物车哟~~
			</view>
			<navigator url="../login/login?backurl=1" class="cartLogin">
				立即登录
			</navigator>
			
		</view>
		<view v-else>
			<view class="cartEmpty" v-if="cartlist.length==0">
				<image src="../../static/image/cartEmpty.png" mode="" class="cartEmptyImg"></image>
				<view class="cartEmptyTip">
					亲,您的购物车空空如也~~
				</view>
				
			</view>
			<view class="cartListBox" v-else>
				<view class="cartList">
					<uni-swipe-action>
					    <uni-swipe-action-item  
						:options="options" 
						@click="delBtn($event,item.id,index)"
						v-for="(item,index) in cartlist" :key="index">
							<view class="cartItem">
								<label @click="checkboxChage(index)">
									<image src="../../static/image/checkbox1.png" mode="" v-if="item.flag"></image>
									<image src="../../static/image/checkbox.png" mode="" v-else></image>
								</label>
								<view class="mainImg">
									<image :src="imgUrl+item.mainimage" mode=""></image>
								</view>
								<view class="cartItemContent">
									<view class="content-title">
										{{item.title}}
									</view>
									<view class="content-attr">
										{{item.attr_0}} {{item.attr_1}}
									</view>
									<view class="content-price">
										¥{{item.price}}
										<view class="numOption">
											<view class="optionBtn" @click="reduce(index)">
												-
											</view>
											<view class="numTxt">
												<input type="text" v-model="item.num" />
											</view>
											<view class="optionBtn" @click="add(index)">
												+
											</view>
										</view>
									</view>
								</view>
							</view>
							</uni-swipe-action-item>
					</uni-swipe-action>
				</view>
				<view class="bottomEmpty">
					
				</view>
				<view class="cartBottom">
					<label @click="allCheckChange">
						<image src="../../static/image/checkbox1.png" mode="" v-if="allCheck"></image>
						<image src="../../static/image/checkbox.png" mode="" v-else></image>
						全选
					</label>
					<view class="cartBottomRight">
						总价:<text style="color: red;">¥{{getAllVal.price}}</text>
						<view class="payBtn" @click="cartBuy">
							去结算<text>(共{{getAllVal.num}}件)</text>
						</view>
					</view>
				</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	import uniSwipeAction from '@/components/uni-swipe-action/uni-swipe-action.vue'
	import uniSwipeActionItem from '@/components/uni-swipe-action-item/uni-swipe-action-item.vue'
	
	export default {
		data() {
			return {
				options:[
				        {
				            text: '删除',
				            style: {
				                backgroundColor: '#ff0309'
				            }
				        }
				      ],
				cartlist:[],
				isLogin:'',
				show:false,
				imgUrl:this.$imgUrl
			}
		},
			
		computed:{
			allCheck(){//全选
				var num=0;
				for(var i=0;i<this.cartlist.length;i++){
					if(this.cartlist[i].flag==1){
						num++
					}
				}
				if(num==this.cartlist.length){
					return true;
				}else{
					return false;
				}
			},
			getAllVal(){//总价
				var num=0;
				var price=0
				for(var i=0;i<this.cartlist.length;i++){
					if(this.cartlist[i].flag==1){
						num+=parseInt(this.cartlist[i].num)
						price+=parseInt(this.cartlist[i].num)*parseFloat(this.cartlist[i].price)
					}
				}
				return {num:num,price:price}
			}
		},
		components:{
			uniSwipeAction,uniSwipeActionItem
		},
		onShow() {//每次进入都调用的生命周期
			this.getData()
		},
		onHide(){//页面离开时候执行，产品数量添加或减少时候，不能每次添加，减少都调用接口，性能不好，要等页面离开才调用接口。
			console.log("onHide")
			
			var temp=[];
			for(var i=0;i<this.cartlist.length;i++){
				temp.push({
					'num':this.cartlist[i].num,
					'attrid':this.cartlist[i].attrid,
					'flag':this.cartlist[i].flag,
				})
			}
			this.$request("/member/savecart",{
				data:JSON.stringify(temp)
			}).then(res=>{
				console.log(res)
			})
		},
		methods: {
			cartBuy(){
				
				var temp=[];
				var tempid=[]
				for(var i=0;i<this.cartlist.length;i++){
					if(this.cartlist[i].flag==1){
						temp.push({num:this.cartlist[i].num,attrid:this.cartlist[i].attrid})
						tempid.push(this.cartlist[i].id)
					}
				}
				// 1,2   [1,2]
				this.$request("/member/delcart",{id:tempid.toString()})
				.then(res=>{
					console.log(res)
				})
				
				
				uni.setStorageSync('orderShop',JSON.stringify(temp))
				this.$back('../order/order')
			},
			
			allCheckChange(){
				if(this.allCheck==true){
					for(var i=0;i<this.cartlist.length;i++){
						this.cartlist[i].flag=0
					}
				}else{
					for(var i=0;i<this.cartlist.length;i++){
						this.cartlist[i].flag=1
					}
				}
			},
			checkboxChage(index){
				this.cartlist[index].flag=!this.cartlist[index].flag
			},
			reduce(index){
				if(this.cartlist[index].num==1){return}
				this.cartlist[index].num--
			},
			add(index){
				if(parseInt(this.cartlist[index].num)>=parseInt(this.cartlist[index].stock))
				{return}
				this.cartlist[index].num++
			},
			getData(){
				
			
				this.$request("/member/cartlist")
				.then(res=>{
					
					if(res.tcode==0){
						this.isLogin=0
					}else{
						this.cartlist=res.data
						console.log(this.cartlist)
						this.isLogin=1
						
					}
					this.show=true
				})
			},
			delBtn(e,id,index){
				if(e.content.text=="删除"){
					uni.showModal({
					    title: '提示',
					    content: '您是否要删除购物车数据',
					    success:  (res)=> {
					        if (res.confirm) {
					            this.cartlist.splice(index,1);
					            this.$request("/member/delcart",{id,id})
					            .then(res=>{
					            	console.log(res)
					            })
					        } else if (res.cancel) {
					            console.log('用户点击取消');
					        }
					    }
					});
					return;
					
					
				}
				console.log(e.content.text)
			}
			
		}
	}
</script>

<style>
.cartEmptyImg{width: 296rpx;height: 296rpx;display: block;margin:154rpx auto 0;}
.cartEmptyTip{text-align: center;font-size: 28rpx;color: #999;line-height: 160rpx;}
.cartLogin{width: 315rpx;height: 80rpx;color: #fff;background: #23baef;
font-size: 28rpx;line-height: 80rpx;text-align: center;margin:0 auto;}

.cartItem{display: flex;padding:35rpx 20rpx 30rpx;border-top:1rpx solid #e5e5e5}
.cartItem label image{width: 30rpx;height: 30rpx;}
.mainImg image{width: 200rpx;height: 200rpx;}
.cartItem checkbox{transform: scale(0.7);}
.cartItem label{display: flex;align-items: center;}
.cartItemContent{flex: 1;}
.content-title{font-size: 28rpx;line-height: 40rpx;height: 80rpx;
overflow: hidden; 
text-overflow: ellipsis;
display: -webkit-box;
-webkit-line-clamp: 2; 
-webkit-box-orient: vertical;}
.content-attr{font-size: 24rpx;color: #999;line-height: 40rpx;}
.content-price{height: 80rpx;display: flex;color: red;
align-items: center;font-size: 28rpx;justify-content: space-between;}
.numOption{display: flex;width: 154rpx;border:1rpx solid #e5e5e5;}
.numOption .optionBtn{width: 44rpx;text-align: center;font-size:28rpx;color: #666;}
.numOption .numTxt{flex:1;border-left: 1rpx solid #e5e5e5
;border-right:1rpx solid #e5e5e5;}
.numOption .numTxt input{width: 100%;font-size:28rpx;text-align: center;color: #666;}
.cartBottom{position: fixed;bottom: var(--window-bottom);
width: 100%;background: #fff;font-size: 28rpx;border-top:1rpx solid #e5e5e5;
height: 106rpx;display: flex;justify-content: space-between;align-items: center;}
.cartBottom label image{width: 30rpx;height: 30rpx;margin:0 10rpx 0 20rpx}
.cartBottomRight{display: flex;align-items: center;}
.cartBottomRight .payBtn{width: 230rpx;height: 80rpx;
background: #0bbaef;color: #fff;margin:0 20rpx;border-radius: 80rpx;line-height: 80rpx;text-align: center;}
.cartBottomRight .payBtn text{font-size: 24rpx;}
.bottomEmpty{height: var(--window-bottom);;}
</style>
