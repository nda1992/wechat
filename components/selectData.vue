<template>
	<view class="select-data-container">
		<view class="select-data-container-tip">
			<text style="margin: 25rpx 0;">{{ tip }}(单位:元)</text>
		</view>
		<view class="select-data-container-show">
			<u-table>
				<u-tr>
					<u-th>项目</u-th>
					<u-th>金额</u-th>
				</u-tr>
				<u-tr v-for="item in showSelectData">
					<u-td>{{ item.name }}</u-td>
					<u-td align="left">{{ item.value }}</u-td>
				</u-tr>
			</u-table>
		</view>
	</view>
</template>

<script>
	export default {
		name:"selectData",
		props: {
			showSelectData: {
				type: Array,
				default: () => {
					return []
				}
			},
			tip: {
				type: String,
				default: () => {
					return ''
				}
			}
		},
		data() {
			return {
				
			};
		}
	}
</script>

<style lang="scss" scoped>
	.select-data-container {
		.select-data-container-tip {
			font-size: 30rpx;
			font-weight: bold;
			display: flex;
			justify-content: center;
			align-items: center;
		}
		.select-data-container-show {
			margin-bottom: 20rpx;
		}
	}
</style>
