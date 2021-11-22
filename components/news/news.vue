<template>
	<view>
		<newsBox v-for="item in newsList" :key="item.id" :newsItem="item.item" />
	</view>
</template>

<script>
import newsBox from '@/components/news/newsBox/index.vue'
export default {
	name:"news",
	components: {
		newsBox
	},
	data() {
		return {
			listQuery: {
				limit: 10,
				page: 1,
				role: 'admin'
			}
		}
	},
	onLoad() {
		this.getnewsList()
	},
	methods: {
		getnewsList() {
			const that = this
			uni.request({
				url: 'http://localhost:3000/news/getnewsList',
				method: 'GET',
				header: {token: 'token'},
				data: that.listQuery,
				success(res) {
					const { items, page } = res.data
					console.log(items);
					console.log(page); 
				}
			})
		}
	}
}
</script>

<style>

</style>
