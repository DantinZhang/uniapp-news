<template>
	<view class="user">
		<view class="nav">
			<image src="../../static/images/history.png" mode="aspectFit"></image>
			<text>浏览历史</text>
		</view>
		
		<view class="content">
			<News 
			v-for="(history,index) in historyArr" 
			@click.native="goDetail(history)" 
			:key="index"
			:item="history"
			></News>
		</view>
		
		<view class="nodata" v-if="!historyArr.length">
			<image src="../../static/images/test.png" mode="widthFix"></image>
			<view style="padding-top:20rpx;">你tm还没浏览的干活</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				historyArr: [],
			};
		},
		//避免每次都要手动刷新，所以放到onShow里
		onShow() {
			//读取本地存储中的浏览历史记录
			this.historyArr = uni.getStorageSync('historyArr').slice(0,10); //slice截取，最多显示10个
		},
		methods: {
			goDetail(historyItem) {
				uni.navigateTo({
					url: `/pages/detail/detail?cid=${historyItem.classid}&id=${historyItem.id}`
				})
			}
		}
	}
</script>

<style lang="scss">
	.user {
		.nav {
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;
			padding:30rpx 0;
			background-color: #F8F8F8 ;
			image {
				height: 200rpx;
				width: 200rpx;
			}
			text {
				font-size: 40rpx;
			}
		}
		.content {
			padding: 30rpx;
		}
		.nodata {
			display: flex;
			flex-direction: column;
			align-items: center;
			
			image {
				width: 80%;
			}
		}
	}
</style>
