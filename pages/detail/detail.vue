<template>
	<view class="detail">
		<view class="title">{{detailData.title}}</view>
		<view class="info">
			<view class="author">编辑：{{detailData.author}}</view>
			<view class="time">发布日期：{{time}}</view>
		</view>
		<view class="content">
			<!-- 识别标签格式的字符串rich-text,当然v-html也可以 -->
			<!-- <rich-text :nodes="detailData.content"></rich-text> -->
			<view v-html="detailData.content"></view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				paramsObj: {},
				detailData: {},
			}
		},
		computed: {
			time() {
				let date = new Date(this.detailData.posttime);
				let Y = date.getFullYear() + '-';
				let M = (date.getMonth()+1 < 10 ? '0'+(date.getMonth()+1) : date.getMonth()+1) + '-';
				let D = date.getDate() + ' ';
				let h = date.getHours() + ':';
				let m = date.getMinutes() + ':';
				let s = date.getSeconds(); 
				let time = Y+M+D+h+m+s;
				return time;
			}
		},
		onLoad(params) {
			console.log(params); //{cid: ---, id:---}
			this.paramsObj = params;
			this.getDetail();
		},
		methods: {
			getDetail() {
				uni.request({
					url: 'https://ku.qingnian8.com/dataApi/news/detail.php',
					data: {
						cid: this.paramsObj.cid,
						id: this.paramsObj.id
					},
					//其实可以直接写成这样 data: this.paramsObj
					success: res => {
						console.log(res.data);
						//解决小程序中的图片展示超出屏幕问题，使用正则替换
						res.data.content = res.data.content.replace(/<img/gi,"<img style='max-width:100%'");
						this.detailData = res.data;
						
						//跳转后更改顶部标题
						uni.setNavigationBarTitle({
							title: res.data.title
						})
					}
				})
			}
		}
	}
</script>

<style lang="scss">
	.detail {
		padding: 30rpx;

		.title {
			font-size: 50rpx;
			color: #333;
		}

		.info {
			background-color: #F6F6F6;
			padding: 20rpx;
			font-size: 30rpx;
			margin: 40rpx 0;
			color: #666;
			display: flex;
			justify-content: space-between;
		}
		
		.content {
			// H5端把所有img都自适应(小程序无效)
			/deep/ img {
				width: 100%;
			}
		}
	}
</style>
