<template>
	<!-- 首页 -->
	<view class="index-container">
		<view class="index-swiper">
			123
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list: [{
						text: '点赞',
						color: 'blue',
						fontSize: 28
						}, {
							text: '分享'
						}, {
							text: '评论'
						}],
				show: true
			}
		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style lang="scss" scoped>
	.index-container {
		.index-swiper {
			color: $u-type-primary;
		}
	}
</style>
