<template>
	<!-- 显示每条新闻的组件 -->
	<view class="newsbox">
		<u-card
		margin="'20rpx'"
		padding="20"
		:title-size="25"
		:title="`${newsItem.createTime} | ${newsItem.category} | ${newsItem.deptName}`"
		:sub-title="`点击数:${newsItem.clickNum}`"
		:sub-title-size="25"
		title-color="#88ada6"
		sub-title-color="#88ada6"
		box-shadow="0 2px 12px 0 rgba(0, 0, 0, 0.1)"
		@click="getCardNewsById(newsItem.id)">
			<view class="newsbox-body" slot="body">
				<image :src="newsItem.url" mode="aspectFill" style="width: 120rpx; height: 120rpx;"></image>
				<view style="margin-left: 15rpx;">{{ newsItem.title }}</view>
			</view>
		</u-card>
	</view>
</template>

<script>
	export default {
		name: 'newsBox',
		props: {
			newsItem: {
				type: Object,
				default: () => {
					return {}
				}
			}
		},
		methods: {
			getCardNewsById(id) {
				this.$emit('getCardNewsById', { id: id })
			}
		}
	}
</script>

<style lang="scss" scoped>
	.newsbox {
		margin-bottom: 15rpx;
		.newsbox-body {
			image {
				width: 120rpx;
				flex: 0 0 120rpx;
				height: 120rpx;
				border-radius: 8rpx;
				margin-right: 12rpx;
			}
			display: flex;
			justify-content: flex-start;
			align-items: center;
		}
	}
</style>
