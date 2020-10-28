<template>
	<view class="content">
		
		<!-- 顶部导航 -->
		<MyHeader :content="recommend_cate"></MyHeader>
		<!-- 首页轮播 -->
		<IndexSwiper :content="banner" 
		swiperHight="390rpx"
		activBtn="#fff"
		width="100%" height="390rpx"
		></IndexSwiper>
		<!-- 首页服务和商品导航 -->
		<IndexService :active="active" :icon="icon"></IndexService>
		<!-- 首页广告 -->
		<IndexAD :content="ad"></IndexAD>
		<!-- 楼层一 -->
		<view class="floor" v-for="(item,index) in floor" :key="index">
			<Title :name="item.content[0].title"></Title>
			<template v-if="item.content.length==1">
				<view class="adImg">
					<image :src="imgUrl+item.content[0].img" mode=""></image>
				</view>
			</template>
			<template v-else>
				<indexTable :content="item.content"></indexTable>
			</template>
			
			
			<shopList :content="item.product"></shopList>
			
		</view>
		
	</view>
</template>

<script>
	import MyHeader from '../../components/header.vue'
	import IndexSwiper from '../../components/indexSwiper.vue'
	import IndexService from '../../components/indexService.vue'
	import IndexAD from "../../components/indexAD.vue"
	import Title from "../../components/title.vue"
	import shopList from '../../components/shopList.vue'
	import indexTable from '../../components/indexTable.vue'
	export default {
		data() {
			return {
				title: 'Hello',
				banner:[],//轮播图
				recommend_cate:[],//顶部推荐分类
				active:[],//服务分类
				icon:[],//分类导航
				ad:[],//首页广告
				floor:[],//楼层信息
				imgUrl:this.$imgUrl
			}
		},
		onTabItemTap(e) {
			
			if(e.index==3){
				uni.switchTab({
					url:"../cate/cate"
				})
			}
			
			
		},
		components:{
			MyHeader,IndexSwiper,IndexService,IndexAD,Title,shopList,indexTable
		},
		onLoad() {
			console.log(this.imgUrl)
			this.getData();
			console.log(getApp().globalData.text)
		},
		methods: {
			getData(){
				uni.request({
				    url: this.apiUrl+'/index', //仅为示例，并非真实接口地址。
				    success: (res) => {
				        // console.log(res.data.data);
						var data=res.data.data;
						this.recommend_cate=data.recommend_cate
						this.banner=data.banner
						this.active=data.active
						this.icon=data.icon
						this.ad=data.floor
						this.floor=data.cate
						console.log(this.floor)
				    }
				});
			}
		}
	}
</script>

<style>
	page{background: #f4f4f4;}
	.adImg{height: 344rpx;width: 100%;}
	.adImg image{width: 100%;height: 344rpx;}
	
</style>
