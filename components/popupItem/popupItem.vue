<template>
	<view class="popupItem-main">
		<uni-collapse :accordion="true">
			<uni-collapse-item 
			v-for="(item, index) in menuList" 
			:key="index" 
			:title="item.parent.name" 
			:show-animation="true">
			<view v-for="(subitem,subindex) in item.children" class="content" :key="subindex">
			<text class="content-sub" @tap='gotoPage(subitem.index, subitem.name)'>{{subitem.name}}</text>
			</view>
			</uni-collapse-item>
		</uni-collapse>
	</view>
</template>

<script>
	export default {
		name:"popupItem",
		props: {
			menuList: {
				type: Array,
				default: () => {
					return []
				}
			}
		},
		methods: {
			gotoPage(index, name) {
				this.$emit('gotoPage', { index: index, name: name })
			}
		}
	}
</script>

<style lang="scss" scoped>
.popupItem-main {
	.content {
		padding: 6rpx 0;
		.content-sub {
			background: #fffffc;
			height: 35rpx;
			width: 100%;
			display: flex;
			align-items: center;
			padding-left: 25rpx;
			font-size: 25rpx;
		}
	}
}
</style>
