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
	import {parseTime} from '../../utils/dateformat.js';
	export default {
		data() {
			return {
				paramsObj: {},
				detailData: {},
				historyArr: uni.getStorageSync('historyArr') || [], //用来存储浏览数据
			}
		},
		computed: {
			time() {
				//时间格式化一下
				return parseTime(this.detailData.posttime);
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
						
						//请求到数据说明已经阅读过，就把相应的信息存到本地存储
						this.saveHistory(res.data);
					}
				})
			},
			saveHistory(newsData) {
				let item = {
					id: newsData.id,
					classid: newsData.classid,
					title: newsData.title,
					picurl: newsData.picurl,
					readtime: parseTime(Date.now()) //阅读过后就添加属性
				}
				//看一下是否已经添加过了，防止重复添加
				let addedIndex = this.historyArr.findIndex(el => {
					return el.id === newsData.id;
				})
				if(addedIndex !== -1) {
					this.historyArr.splice(addedIndex, 1);
				} 
				this.historyArr.unshift(item); //精简一下代码
				//放到本地存储中
				uni.setStorageSync('historyArr', this.historyArr);
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
			font-size: 28rpx;
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
