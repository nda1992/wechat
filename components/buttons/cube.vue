<template>
	<!-- 15个按钮的组件 -->
	<view class="cube">
		<view class="cube-main" @click="gotoTargePage(index, content)">
			<view class="cube-main-children">
				<image :src="icon"></image>
				<text>{{ content }}</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: 'Cube',
		props: {
			icon: {
				type: String,
				default: () => {
					return ''
				}
			},
			content: {
				type: String,
				default: () => {
					return ''
				}
			},
			index: {
				type: String,
				default: () => {
					return ''
				}
			}
		},
		methods: {
			gotoTargePage(index, content) {
				const temp = { index: index, category: content }
				this.$emit('gotoTargePage', temp)
			}
		}
	}
	
</script>

<style lang="scss" scoped>
	.cube {
		.cube-main {
			.cube-main-children {
				image {
					height: 50rpx;
					width: 50rpx;
				}
				text {
					margin-top: 8rpx;
				}
				font-size: 20rpx;
				height: 130rpx;
				width: 130rpx;
				display: flex;
				flex-direction: column;
				justify-content: center;
				align-items: center;
				border: dashed #E4E7ED 0.5px;
				border-radius: 9rpx;
				// box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1)
			}
		}
	}
</style>
