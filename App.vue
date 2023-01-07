<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		}
	}
</script>

<style>
	view {
		box-sizing: border-box;
	}
	
	/* 去掉H5端导航的滚动条 */
	::-webkit-scrollbar{
		display: none;
	}
</style>
