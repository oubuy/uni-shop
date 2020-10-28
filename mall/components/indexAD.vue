<template>
	<view class="indexAD">
		<view class="indexADleft">
			<template v-if="content.length>0">
				<navigator url="#" hover-class="none">
					<image :src="imgUrl+content[0].image" mode=""></image>
				</navigator>
			</template>
		</view>
		<view class="indexADright">
			<view class="indexADrightItem">
				<template v-if="content.length>0">
					<navigator url="#" hover-class="none">
						<image :src="imgUrl+content[1].image" mode=""></image>
					</navigator>
				</template>
			</view>
			<view class="indexADrightItem">
				<template v-if="content.length>0">
					<navigator url="#" hover-class="none">
						<image :src="imgUrl+content[2].image" mode=""></image>
					</navigator>
				</template>
			</view>
		</view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				imgUrl:this.$imgUrl
			}
		},
		props:['content']
	}
</script>

<style>
	.indexAD{display: flex;border-top:20rpx solid #f4f4f4;}
	.indexADleft{width: 50%;height: 520rpx;}
	.indexADleft image{width: 100%;height: 520rpx;}
	.indexADright{width: 50%;}
	.indexADright image{width: 100%;height: 260rpx;;}
	.indexADrightItem{height:260rpx ;}
</style>
