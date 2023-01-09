<template>
	<view class="home">
		<!-- 导航部分的横向滚动，注意要换行white-space: nowrap; -->
		<scroll-view scroll-x="true" class="scrollNav">
			<view class="item" 
			v-for="(item,index) in navArr" 
			:class="{active: curIndex === index}"
			@click="changeIndex(index,item.id)"
			:key="item.id">{{item.classname}}</view>
		</scroll-view>

		<!-- 下面的新闻部分 -->
		<view class="content">
			<News 
			v-for="(item,index) in newsArr" 
			:key="item.id" 
			@click.native="goDetail(item)" 
			:item="item" />
		</view>
		
		<!-- 没有数据时展示下面的图片 -->
		<view class="nodata" v-if="!newsArr.length">
			<!-- mode控制以宽度还是长度为基准 -->
			<image src="../../static/images/test.png" mode="widthFix"></image>
			<view style="margin-top: 30rpx;">这里tmd没数据啊兄弟</view>
		</view>
		
		<!-- 数据加载中的提示，增加用户体验 -->
		<view class="loading" v-if="newsArr.length">
			<view v-show="loading === 1">数据加载中......</view>
			<view v-show="loading === 2">没有更多数据了~~</view>
		</view>
	</view>
</template>

<script>
	import {parseTime} from '../../utils/dateformat.js';
	export default {
		data() {
			return {
				curIndex: 0, //代表默认选中第一个
				navArr: [],
				newsArr: [],
				currentPage:1,  //当前页码
				loading:0,   //0默认不显示 1表示加载中  2表示没有数据
			}
		},
		onLoad() {
			this.getNavData();
			this.getNewsData();
		},
		//2.触底加载更多数据
		onReachBottom() {
			//如果触底时已经没有数据了，就不再发请求了，一直显示没数据
			if(this.loading === 2) return;  //这行如果不写，每次触底都会提示加载并发请求，不太好
			this.currentPage++;
			this.loading = 1;  //触底时显示数据加载中
			this.getNewsData();
		},
		methods: {
			//点击导航切换，一定要清空上一次请求残留的更改信息，否则会有bug
			changeIndex(index,navId) {
				this.curIndex = index;
				this.currentPage = 1;  //重置当前页数（其他类触底改变了该值）
				this.newsArr = [];  //重置数组，否则点击切换会出现继续拼接的情况
				this.loading = 0;  //切换时去掉数据加载中
				this.getNewsData(navId);  //请求对应的数据
			},
			
			//跳转到详情页
			goDetail(item) {
				console.log('新闻详情数据',item);
				uni.navigateTo({
					url:`/pages/detail/detail?cid=${item.classid}&id=${item.id}`
				});
			},
			
			//1.1获取导航列表数据
			getNavData() {
				uni.request({
					url:"https://ku.qingnian8.com/dataApi/news/navlist.php",
					success:res => {
						console.log(res);
						this.navArr = res.data;
					}
				})
			},
			
			//1.2获取新闻列表数据，接收参数栏目id，默认请求第一类
			getNewsData(navId=50) {
				uni.request({
					url:"https://ku.qingnian8.com/dataApi/news/newslist.php",
					//传参
					data: {
						cid:navId, //拿着栏目id发请求
						page:this.currentPage
					},
					success:res => {
						console.log(res.data);
						if(res.data.length === 0) {
							this.loading = 2;  //如果下拉请求不到数据了，就提示
						}
						//两个数组拼接的两种写法
						// this.newsArr = this.newsArr.concat(res.data);
						this.newsArr = [...this.newsArr,...res.data];
					}
				});
				
			}
		}
	}
</script>

<style lang="scss">
	.home {
		.scrollNav {
			position: fixed;
			top: var(--window-top); /*h5端的操作*/
			left: 0;
			z-index: 99;
			height: 100rpx;
			line-height: 100rpx;
			background-color: #F7F8FA;
			white-space: nowrap;

			.item {
				display: inline-block;
				font-size: 40rpx;
				padding: 0 30rpx;
				color: #333;
				&.active {
					 color: #31C27C;
				}
			}
		}
		
		.content {
			padding: 30rpx;
			padding-top: 100rpx;
		}
		
		.nodata {
			display: flex;
			flex-direction: column;
			align-items: center;
			image {
				width: 660rpx;
			}
		}
		
		.loading {
			font-size: 26rpx;
			color: #888;
			text-align: center;
			line-height: 2em;
		}
	}
</style>
