<template>
	<view class="index">
		<cube
		 v-for="item in menuLists"
		 :key="item.id"
		 :icon="item.icon"
		 :content="item.content.name"
		 :index="item.content.index"
		 @gotoTargePage="gotoTargePage"
		 />
	</view>
</template>

<script>
import cube from '@/components/buttons/cube.vue'
export default {
	name: 'index',
	components: {
		cube
	},
	props: {
		menuLists: {
			type: Array,
			default: () => {
				return []
			}
		}
	},
	methods: {
		gotoTargePage(temp) {
			this.$emit('gotoTargePage',temp)
		}
	}
}
</script>
<style lang="scss" scoped>
	.index {
		display: flex;
		flex-wrap: wrap;
		justify-content: space-around;
		align-items: center;
		height: 450rpx;
	}
</style>