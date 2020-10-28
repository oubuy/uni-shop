<script>
	export default {
		globalData: {  
		    text: 'text'  
		},
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		},
		
	}
</script>

<style>
	/*每个页面公共css */
		@font-face {
		     font-family: 'iconfont';
		     src: url('~@/static/font/iconfont.ttf') format('truetype');
		 }
		 .iconfont {
		     font-family: iconfont;
		 }
		 i{font-style: normal;}
		 *{font-family: "微软雅黑";}
		input,textarea{font-size: 28rpx;}
		
</style>
