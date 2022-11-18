<template>
	<view class="user">
		<view class="nav">
			<image src="../../static/images/history.png" mode=""></image>
			<text>浏览历史</text>
		</view>
		
		<view class="content">
			<News @click.native="goDetail" v-for="(item,index) in 10" :key="index"></News>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			};
		},
		methods: {
			goDetail() {
				uni.navigateTo({
					url: "/pages/detail/detail"
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
	}
</style>
