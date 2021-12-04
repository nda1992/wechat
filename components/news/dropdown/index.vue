<template>
	<view class="dropdown-main">
		<u-dropdown>
			<u-dropdown-item v-model="value" title="排序" :options="options"></u-dropdown-item>
		</u-dropdown>
	</view>
</template>

<script>
	export default {
		name: 'newsdropdown',
		data() {
			return {
				value: 1,
				options: [
					{label: '浏览次数',value: 1},{label: '发布日期',value: 2}
				]
			}
		}
	}
</script>

<style>
</style>
