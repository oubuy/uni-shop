<template>
	<view>
		<IndexSwiper activBtn="#0bc9fd" :content="detail.image" swiperHight="525rpx" width="470rpx" height="470rpx"></IndexSwiper>
		<view class="shopInfo">
			<view class="shop-title">
				<label v-if="detail.tag==1">新品</label>
				<label v-if="detail.tag==2">热销</label>
				<label v-if="detail.tag==3">促销</label>
				{{detail.title}}
			</view>
			<view class="shop-summary">
				{{detail.summary}}
			</view>
			<view class="shop-price-sale">
				<view class="shop-price"><text>￥</text>{{detail.price}}<label>￥{{detail.market_price}}</label></view>
				<view class="shop-sale">月销量:{{detail.sale}}件</view>
			</view>
			<view class="shop-assure" @click="showService">
				<view class="shop-assure-item" 
				v-for="(item,index) in detail.servicelist" :key="index">
					<template v-if="index<2">
						<i class="iconfont">&#xe60d;</i>{{item.title}}
					</template>
				</view>
				
				<view class="shop-assure-more">
					<i class="iconfont">&#xe60b;</i>
				</view>
			</view>
			
		</view>
		<view class="shopAttr" @click="showAttr(2)">
			
			<label>规格</label><text>{{attrTxt}}</text>
			
			<i class="iconfont">&#xe60b;</i>
			
		</view>
		
		
		<view class="shopTab">
			<view class="shopTabItem" @click="tabChange(0)"
			:class="{tabActive:tab==0}">
				图文详情
			</view>
			<view class="shopTabItem"  @click="tabChange(1)"
			:class="{tabActive:tab==1}"
			>
				规格参数
			</view>
		</view>
		<view class="content"  v-if="tab==0">
			<!-- <image src="../../static/image/d1.jpg" mode="widthFix"></image>
			<image src="../../static/image/d2.jpg" mode="widthFix"></image>
			<image src="../../static/image/d3.jpg" mode="widthFix"></image>
		 -->
			<parse :content="detail.content"
			:imageProp="{'domain':'www.mall.com'}"
			></parse>
		</view>
		<view class="shopSpecs" v-if="tab==1">
			<view class="specsTitle">
				基础信息
			</view>
			<view class="specsContent" v-for="(item,index) in detail.spec"
			:key="index">
				<view class="specsContentLeft">
					{{item.specname}}
				</view>
				<view class="specsContentRight">
					{{item.specvalue}}
				</view>
			</view>
			
			
		</view>
		<view style="height: 100rpx;width: 100%;"></view>
		<view class="detailBottom">
			<view class="bottomItem">
				<view class="bottomItemIndex">
					<navigator url="../index/index"
					 hover-class="none" open-type="switchTab">
						<image src="../../static/tabbar1.png" mode=""></image>
						<text>首页</text>
					</navigator>
				</view>
				<view class="bottomItemIndex">
					<navigator url="../cart/cart" hover-class="none"
					 open-type="switchTab">
					<image src="../../static/tabbar3.png" mode=""></image>
					<text>购物车</text>
					</navigator>
				</view>
			</view>
			<view class="bottomButton cartButton" @click="showAttr(1)">
				加入购物车
			</view>
			<view class="bottomButton buyButton" @click="showAttr(2)">
				立即购买
			</view>
		</view>
		<detailService :content="detail.servicelist" @close="closeService" v-if="serviceFlag"></detailService>
		<detailAttr
		 :image="detail.mainimage"
		 :price="detail.price"
		 :attr="detail.checkAttr"
		 :attrVal="detail.attr"
		 :type="type"
		 v-if="attrFlag"
		 @close="closeAttr"
		 
		 ></detailAttr>
		
		</view>
</template>

<script>
	import detailService from '../../components/detailService.vue'
	import detailAttr from '../../components/detailAttr.vue'
	import IndexSwiper from '../../components/indexSwiper.vue'
	import parse from '../../components/gaoyia-parse/parse.vue'
	import {mapState} from "vuex"
	export default {
		data() {
			return {
				tab:0,
				serviceFlag:false,//服务弹窗的显示隐藏
				attrFlag:false,//属性弹窗的显示隐藏
				detail:[],
				type:2//1表示加入购物车，2表示立即购买
			}
		},
		onLoad(option) {
			console.log()
			this.getData(option.id)
		},
		computed:{
			...mapState(['attrTxt'])
		},
		beforeDestroy() {
			this.$store.commit("defaultAttr")
		},
		components:{detailService,detailAttr,IndexSwiper,parse},
		methods: {
			//获取详情数据
			getData(id){
				uni.request({
					url:this.apiUrl+"/index/detail/id/"+id,
					success:(res)=>{
						this.detail=res.data.data;
						console.log(this.detail)
					}
				})
			},
			tabChange(index){
				this.tab=index
			},
			//显示服务弹窗
			showService(){
				this.serviceFlag=true;
			},
			//关闭服务弹窗
			closeService(){
				this.serviceFlag=false;
			},
			//显示属性弹窗
			showAttr(type){
				
				this.type=type
				this.attrFlag=true;
			},
			//关闭属性弹窗
			closeAttr(){
				this.attrFlag=false
			}
		}
	}
</script>

<style>
.detailSwiper{width: 100%;height: 525rpx;}
.detailSwiper image{width: 470rpx;height: 470rpx;display: block;margin:0 auto;}
.shopInfo{padding:0 30rpx;border-top: 1rpx solid #e9e9e9;}
.shop-title{line-height: 40rpx;font-size: 28rpx;color: #000;padding-top:24rpx;}
.shop-title label{display: block;width: 64rpx;height: 40rpx;color: #fff;background: #18bfe1;float:left;
text-align: center;margin-right: 10rpx;;}
.shop-summary{font-size:24rpx;color: #9a9a9a;line-height: 35rpx;padding-top:10rpx;;}
.shop-price-sale{line-height: 83rpx;height: 83rpx;display: flex;justify-content: space-between;
color: #999;font-size: 28rpx;border-bottom: 1rpx solid #e9e9e9;}
.shop-price{font-size:36rpx ;color: #f0415f;}
.shop-price label{font-size: 28rpx;color: #999;text-decoration: line-through;
margin-left:10rpx;}
.shop-assure{height: 80rpx;line-height: 80rpx;overflow: hidden;}
.shop-assure-item {font-size: 24rpx;color: #999;float:left;
margin-right: 20rpx;display: flex;}
.shop-assure-item i{font-size: 30rpx;color: #00c3f5;margin-right: 5rpx;}
.shop-assure-more{float:right;color: #999;}
.shopAttr{line-height: 90rpx;padding:0 30rpx;border-top: 20rpx solid #f7f7f7;
border-bottom: 20rpx solid #f7f7f7;color: #999;}
.content image{width: 100%;display: block;}
.shopAttr label{font-size: 28rpx;}
.shopAttr text{color: #000;margin-left:24rpx;font-size: 28rpx;}
.shopAttr i{float:right;color: #999;}
.shopTab{height: 80rpx;display: flex;align-items: center;border-bottom: 1rpx solid #e5e5e5;}
.shopTabItem{width: 50%;text-align: center;font-size: 28rpx;font-weight: 700;}
.shopTabItem.tabActive{color: #00C3F5;}
.shopSpecs{margin:20rpx 30rpx ;border:1rpx solid #e9e9e9}
.specsTitle{line-height: 75rpx;height: 75rpx;background: #e9e9e9;
font-size: 28rpx;color: #000;text-indent: 30rpx;
}
.specsContent{display: flex;line-height: 80rpx;border-bottom: 1rpx solid #e9e9e9;}
.specsContentLeft{width: 200rpx;text-align: center;font-size: 24rpx;color: #999;}
.specsContentRight{flex:1;text-align: center;font-size: 24rpx;color: #000;}

.detailBottom{width: 100%;height: 100rpx;background: #fff;
position: fixed;bottom:0;display: flex;}
.bottomButton{width: 275rpx;color: #fff;font-size: 28rpx;}
.bottomItem{flex:1;display: flex;}
.bottomItemIndex{width: 50%;}
.bottomItemIndex image{width: 40rpx;height: 40rpx;
display: block;margin:10rpx auto 0;}
.bottomItemIndex text{color: #999;font-size: 24rpx;
text-align: center;display: block;height: 40rpx;line-height: 40rpx;}
.cartButton{background: #0ebcef;text-align: center;line-height: 100rpx;}
.buyButton{background: #017cfe;text-align: center;line-height: 100rpx;}


</style>
