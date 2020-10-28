<template>
	<view class="attrDialog">
		<view class="bg"></view>
		<view class="attrDialogContent">
			<view class="contentTop">
				<view class="contentImg">
					<image :src="imgUrl+image" mode=""></image>
				</view>
				
				<view class="contentTopTxt">
					<text class="price">¥{{currentprice}}</text>
					<text class="attr">{{checkAttr}}</text>
				</view>
				<i class="iconfont" @click="closeDialog">&#xe62b;</i>
			</view>
			<view class="attrList">
				<view class="attrItem" v-for="(item,index) in attr">
					<view class="attrItemTit">
						{{item.attrname}}
					</view>
					<view class="attrItemVal">
						<!-- activeAttr -->
						<view class="attrValItem" 
						v-for="(child,childindex) in item.value" 
						:key="childindex"
						:class="{activeAttr:valindex[index]==childindex}"
						@click="attrCheck(index,childindex)"
						>
							{{child}}
						</view>
						
					</view>
				</view>
				
				<view class="attrItem">
					<view class="attrItemTit">
						数量
					</view>
					<view class="attrItemVal">
						<view class="shopNum">
							<view class="button" @click="reduce">-</view>
							<view><input type="text" v-model="num" disabled/></view>
							<view class="button" @click="add">+</view>
						</view>
						<text class="stock">(库存：{{stock}}件)</text>
					</view>
					
				</view>
			</view>
		</view>
		<view class="attrButton" @click="attrButton">
			{{buttonTxt}}
		</view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				valindex:[],
				checkAttr:'',
				currentprice:'',
				stock:'',
				num:1,
				buttonTxt:"",
				attrid:'',
				imgUrl:this.$imgUrl
			}
		},
		
		
		mounted() {
				//[0,0],[1,1]
			if(this.$store.state.attrVal.length==0){
				for(var i=0;i<this.attr.length;i++){
					this.valindex.push(0)
				}
			}else{
				this.valindex=this.$store.state.attrVal
			}
				
			
			
			
			this.checkAttr=this.getVal().toString()
			this.getPrcie(this.getVal())
			
			if(this.type==1){
				this.buttonTxt="加入购物车"
			}else{
				this.buttonTxt="立即购买"
				
			}
		},
		methods:{
			attrButton(){
				
				if(this.type==1){
					//跳转到购物车
					
					this.$request('/member/addcart',{
						"num":this.num,"attrid":this.attrid
					}).then(res=>{
						if(res.code==1){
							this.$back('../cart/cart',2)
						}
					})
				}else{
					//立即购买
					var orderShop=[{"num":this.num,"attrid":this.attrid}]
					uni.setStorageSync("orderShop",JSON.stringify(orderShop))
					this.$back('../order/order')
				}
			},
			reduce(){
				if(this.num==1){return;}
				this.num--
			},
			add(){
				if(parseInt(this.num)>=parseInt(this.stock)){return;}
				this.num++
			},
			
			closeDialog(){
				this.$emit('close')
				console.log(this.valindex,this.checkAttr)
				this.$store.commit('setAttr',{
					"attrVal":this.valindex,
					"attrTxt":this.checkAttr
				})
			},
			attrCheck(index,childindex){
				// this.valindex[index]=childindex
				//数组是引用类型
				//[0,0]
				this.valindex.splice(index,1,childindex)
				
				var valcheck=this.getVal();
				this.checkAttr=valcheck.toString();
				this.getPrcie(valcheck);
				
			},
			getPrcie(valcheck){
				var temp=[]
				for(var i=0;i<this.attrVal.length;i++){
					if(this.attrVal[i]['attr_0']==valcheck[0]&&this.attrVal[i]['attr_1']==valcheck[1]){
						temp.push(this.attrVal[i]);
						
					}
				}
				this.currentprice=temp[0].price
				this.stock=temp[0].stock
				this.attrid=temp[0].id
			
			},
			getVal(){
				var temp=[]
				for(var i=0;i<this.attr.length;i++){
					for(var j=0;j<this.attr[i].value.length;j++){
						
						temp.push(this.attr[i].value[this.valindex[i]])
						break
					}
				}
				return temp;
			}
			
		},
		props:['image','price','attrVal','attr','type']
	}
</script>

<style>
	.attrDialog{position: fixed;top:0;left:0;bottom:0;right:0;}
	.attrDialog .bg{background: rgba(0,0,0,0.5);height: 100%;width: 100%;}
	.attrDialogContent{height: 1070rpx;position: absolute;bottom:0;
	width: 100%;background: #fff;}
	.contentTop{height: 200rpx;position: relative;border-bottom: 1rpx solid #e5e5e5;}
	.contentTop i{position: absolute;top: 25rpx;right:30rpx;}
	.contentImg{position: absolute;top:-40rpx;left:30rpx;
	width: 210rpx;height: 210rpx;border: 1rpx solid #e5e5e5;background: #fff;}
	.contentTop image{width: 100%;height: 100%;}
	.contentTopTxt{margin-left:280rpx;}
	.contentTopTxt .price{height: 44rpx;color: #f0415f;font-size: 36rpx;
	display: block;line-height: 44rpx;padding-top:24rpx;}
	.contentTopTxt .attr{font-size: 24rpx;color: #000;line-height: 40rpx;
	height: 40rpx;}
	.attrList{padding: 0 30rpx;;}
	.attrItemTit{color: #999;line-height: 70rpx;font-size: 28rpx;}
	.attrItem{height: auto;overflow: hidden;}
	.attrValItem{padding:0 30rpx;border:1rpx solid #e5e5e5;height: 70rpx;
	line-height: 70rpx;float:left;font-size: 24rpx;color: #666;margin-right: 30rpx;}
	.attrValItem.activeAttr{color: #00c3f5;border-color:#00c3f5;}
	.stock{height: 70rpx;line-height: 70rpx;font-size: 24rpx;color: #999;margin-left:20rpx}
	.shopNum{width: 240rpx;height: 70rpx;border:1rpx solid #e5e5e5;
	display: flex;float:left;}
	.shopNum .button{width: 70rpx;line-height: 70rpx;text-align: center;line-height: 70rpx;}
	.shopNum input{border-right:1rpx solid #e5e5e5;border-left:1rpx solid #e5e5e5;width: 100rpx;
	height: 70rpx;line-height: 70rpx;text-align: center;font-size: 24rpx;}
	.attrButton{height: 90rpx;line-height: 90rpx;position: absolute;bottom:0;
	background: #0ebcef;font-size: 28rpx;color: #fff;width: 100%;text-align: center;}
	
</style>
