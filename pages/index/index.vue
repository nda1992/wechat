<template>
	<!-- 首页 -->
	<view class="index-container">
		<view class="index-swiper">
			<u-swiper :list="list" :effect3d="true" title @click='goto' mode="rect"></u-swiper>
		</view>
		<u-notice-bar mode="horizontal" :list="text" color="#006e54"></u-notice-bar>
		<view class="index-content">
			<view class="index-content-header">
				<view class="index-content-header-list" @click="openListDraw">
					<image src="../../static/images/buttons/list.png"></image>
				</view>
				<view class="index-content-header-text">
					<text>南华大学附属第一医院欢迎您~</text>
				</view>
				<view class="index-content-header-time">
					<text>{{ date }}</text>|<text>{{ week }}</text>
				</view>
			</view>
			<u-divider :half-width='350' :margin-top="10" :margin-bottom="10">快捷按钮</u-divider>
			<view class="index-content-buttons">
				<u-button></u-button>
			</view>
			<u-divider :half-width='350' :margin-top="10" :margin-bottom="10">热点新闻</u-divider>
			<view class="index-content-news">
				
			</view>
		</view>
		<view class="draw">
			<u-popup v-model="showDraw" width="450rpx">
				<view>出淤泥而不染，濯清涟而不妖</view>
			</u-popup>
		</view>
	</view>
</template>

<script>
import moment from 'moment'
import getWeek from '@/static/js/getWeek.js'
export default {
	data() {
		return {
			text: ['挂号请关注《南华大学附属第一医院公众号》进行操作...'],
			list: [{
				image: 'https://cdn.uviewui.com/uview/swiper/1.jpg',
				title: '昨夜星辰昨夜风，画楼西畔桂堂东',
				newsid: 1
			},
			{
				image: 'https://cdn.uviewui.com/uview/swiper/2.jpg',
				title: '身无彩凤双飞翼，心有灵犀一点通',
				newsid: 2
			},
			{
				image: 'https://cdn.uviewui.com/uview/swiper/3.jpg',
				title: '谁念西风独自凉，萧萧黄叶闭疏窗，沉思往事立残阳',
				newsid: 3
			}
		],
		showDraw: false,
		date: undefined,
		week: undefined
		}
	},
	onLoad() {
		this.getNow()
	},
	methods: {
		getNow() {
			this.date = moment().format('YYYY年MM月DD日')
			this.week = getWeek(moment().day())
		},
		goto(index) {
			const newsid = this.list[index].newsid
			console.log(newsid)
		},
		openListDraw() {
			this.showDraw = !this.showDraw
		}
	}
}
</script>

<style lang="scss" scoped>
.index-container {
	.index-swiper {
		
	}
	.index-content {
		padding: 20rpx;
		.index-content-header {
			font-size: 25rpx;
			display: flex;
			justify-content: space-around;
			align-items: center;
			.index-content-header-list {
				image {
					width: 50rpx;
					height: 50rpx;
				}
			}
			.index-content-header-time {
				color: #999999;
			}
		}
	}
}
</style>
