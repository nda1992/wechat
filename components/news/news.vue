<template>
	<view>
		<newsBox v-for="item in newsList" :key="item.id" :newsItem="item.item" />
	<!-- 浏览新闻的组件 -->
		<view class="news-main">
			<view class="news-main-title">{{ item.title }}</view>
			<view class="news-main-info">
				<text>发布时间:{{ item.createTime }}</text>
				<text>来源:{{ item.deptName }}</text>
				<text>浏览次数:{{ item.clickNum }}</text>
			</view>
			<view class="news-main-content">
				<u-parse :html="item.content"></u-parse>
			</view>
		</view>
	</view>
</template>

<script>
import newsBox from '@/components/news/newsBox/index.vue'
export default {
	name:"news",
	components: {
		newsBox
	},
	data() {
		return {
			listQuery: {
				limit: 10,
				page: 1,
				role: 'admin'
			}
		}
	},
	onLoad() {
		this.getnewsList()
	},
	methods: {
		getnewsList() {
			const that = this
			uni.request({
				url: 'http://localhost:3000/news/getnewsList',
				method: 'GET',
				header: {token: 'token'},
				data: that.listQuery,
				success(res) {
					const { items, page } = res.data
					console.log(items);
					console.log(page); 
				}
			})
		}
	}
}
</script>

<style>
	export default {
		name:"news",
		props: {
			item: {
				type: Object,
				default: () => {
					return {}
				}
			}
		}
	}
</script>

<style lang="scss" scoped>
	.news-main {
		margin: 20rpx 0;
		.news-main-title {
			font-size: 30rpx;
			font-weight: bold;
			padding: 0 10rpx;
			margin: 20rpx 0;
		}
		.news-main-info {
			display: flex;
			justify-content: space-around;
			color: #999999;
			font-size: 25rpx;
			padding: 0 25rpx ;
		}
		.news-main-content {
			margin-top: 30rpx;
			padding: 0 35rpx;
		}
	}
</style>
