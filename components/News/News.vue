<!-- 在uniapp中，公共组件不需要注册引入，只要在components目录中，就可以直接写标签用 -->
<template>
	<view class="new">

		<view class="pic">
			<!-- mode控制以宽度还是长度为基准 -->
			<image :src="item.picurl" mode="aspectFill"></image>
		</view>

		<view class="text">
			<view class="title">{{item.title}}</view>
			
			<view class="detail" v-if="!item.readtime">
				<text>{{item.author}}</text>
				<text style="margin-left:15rpx">{{item.hits}}浏览</text>
			</view>
			
			<view class="detail" v-if="item.readtime">
				<text>浏览时间：{{item.readtime}}</text>
			</view>
		</view>

	</view>
</template>

<script>
	export default {
		name: "News",
		onLoad() {
			console.log(this)
		},
		props: {
			//这里接到的props可以修改，因为每个组件接收的值不一样，修改不会相互影响
			item : {
				type: Object,
				default: function() {
					//这里定义为函数，本质和data定义为函数是一样的
					//其实我感觉这里根本不需要给默认值……可能是老师为了讲课吧
					return {
						title: "新闻默认标题",
						author:"奥里给",
						hits:668,
						picurl:"https://t7.baidu.com/it/u=2621658848,3952322712&fm=193&f=GIF",
					}
				}
			}
		},
		data() {
			return {
				
			};
		},
		methods: {
			ok() {
				this.item.hits++;
				console.log(this.item.hits)
			}
		}
	}
</script>

<style lang="scss">
	.new {
		border-bottom: 1px solid #efefef;
		padding: 15rpx 0;
		display: flex;
		justify-content: space-between;

		.pic {
			flex: 1;
			height: 160rpx;
			width: 230rpx;

			// 图片默认是有宽高的，所以要重新定义它
			image {
				width: 100%;
				height: 100%;
			}
		}

		.text {
			flex: 2;
			margin-left: 20rpx;
			display: flex;
			flex-direction: column;
			justify-content: space-between;

			.title {
				font-size: 36rpx;
				color: #333;
				//css3设置超出两行省略号
				text-overflow: -o-ellipsis-lastline;
				overflow: hidden; //溢出内容隐藏
				text-overflow: ellipsis; //文本溢出部分用省略号表示
				display: -webkit-box; //特别显示模式
				-webkit-line-clamp: 2; //行数
				line-clamp: 2;
				-webkit-box-orient: vertical; //盒子中内容竖直排列
			}

			.detail {
				font-size: 28rpx;
				color: #B0B0AE;
			}
		}
	}
</style>
