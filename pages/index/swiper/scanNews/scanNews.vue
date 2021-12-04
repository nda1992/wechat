<template>
	<view>
		<news :item="newsItem" />
		<view class="btn">
			<u-button type="info" @click="back" size="mini">返回</u-button>
		</view>
	</view>
</template>

<script>
import moment from 'moment'
import news from '@/components/news/news.vue'
export default {
	components: {
		news
	},
	data() {
		return {
			newsItem: {}
		}
	},
	onLoad(option) {
		this.getNewsById(option)
	},
	methods: {
		getNewsById({newsid}) {
			const that = this
			uni.request({
				url: 'http://localhost:3000/website/news/getNewsById',
				method: 'GET',
				header: {token: 'token'},
				data: {id: newsid},
				success(res) {
					const { item } = res.data
					const tempTime = moment(item.createTime).format('YYYY-MM-DD hh:mm:ss')
					that.newsItem = {
						title: item.title,
						clickNum: item.clickNum,
						deptName: item.deptName,
						createTime: tempTime,
						content: item.content
					}
				}
			})
		},
		back() {
			uni.navigateBack({
				delta:1
			})
		}
	}
}
</script>
<style lang="scss" scoped>
	.btn {
		margin: 40rpx 0 50rpx 0;
		display: flex;
		justify-content: center;
		align-items: center;
	}
</style>