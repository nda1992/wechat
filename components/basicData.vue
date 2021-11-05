<template>
	<view class="basicData-container">
		<view class="basicData-box">
			<text>{{ showData.name }}</text>
			<text>{{ showData.value }}</text>
		</view>
	</view>
</template>

<script>
	export default {
		name:"basicData",
		props: {
			showData: {
				type: Object,
				default: () => {
					return {}
				}
			}
		},
		data() {
			return {
				
			}
		}
	}
</script>

<style lang="scss" scoped>
	.basicData-container {
		.basicData-box {
			display: flex;
			flex-direction: column;
			align-items: center;
			padding: 15rpx;
			width: 200rpx;
			color: #366092;
			font-weight: bold;
		}
	}
</style>
