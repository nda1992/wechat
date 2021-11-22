<template>
	<!-- 显示每条新闻的组件 -->
	<view class="newsbox">
		<u-card margin="'20rpx'" padding="20" :title-size="25" title="2021-10-12" title-color="#88ada6" box-shadow="0 2px 12px 0 rgba(0, 0, 0, 0.1)">
			<view class="newsbox-body" slot="body">
				<image src="../../../static/logo.png" mode="aspectFill" style="width: 120rpx; height: 120rpx;"></image>
				<view>123</view>
			</view>
		</u-card>
	</view>
</template>

<script>
	export default {
		props: {
			newsItem: {
				type: Object,
				default: () => {
					return {}
				}
			}
		},
		data() {
			return {
				date: '2021-10-12'
			}
		}
	}
</script>

<style lang="scss" scoped>
	.newsbox {
		.newsbox-body {
			display: flex;
			justify-content: space-between;
			align-items: center;
		}
	}
</style>
