<template>
<<<<<<< HEAD
=======
	<view>
		<newsBox v-for="item in newsList" :key="item.id" :newsItem="item.item" />
>>>>>>> 027874b98899deb277d419feddb982fcdd94c32f
	<!-- 浏览新闻的组件 -->
		<view class="news-main">
			<view class="news-main-title">
				<text>{{ item.title }}</text>
			</view>
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
<<<<<<< HEAD
=======
import newsBox from '@/components/news/newsBox/index.vue'
>>>>>>> 027874b98899deb277d419feddb982fcdd94c32f
export default {
	name:"news",
	props: {
		item: {
			type: Object,
			default: () => {
				return {}
			}
		}
	},
	data() {
		return {
			listQuery: {
				limit: 10,
				page: 1,
				role: 'admin'
			}
		}
	}
}
</script>

<<<<<<< HEAD
=======
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

>>>>>>> 027874b98899deb277d419feddb982fcdd94c32f
<style lang="scss" scoped>
	.news-main {
		margin: 20rpx 0;
		.news-main-title {
			font-size: 30rpx;
			font-weight: bold;
			padding: 0 10rpx;
			margin: 20rpx 0;
			display: flex;
			justify-content: center;
			align-items: center;
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
