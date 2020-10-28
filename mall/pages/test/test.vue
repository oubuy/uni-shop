<template>
	<view>
		<button type="primary" @click="buy">立即下单</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			buy(){
				
				var shop=[{"attrid":1,"num":1},{"attrid":2,"num":1}];
				uni.request({
					url:"http://www.mall.com/api/member/order",
					method:"POST",
					data:{
						data:shop,
						price:100,
						paytype:1,
						logistics:1
					},
					success:(res)=> {
						console.log(res)
					}
				})
			}
		}
	}
</script>

<style>

</style>
