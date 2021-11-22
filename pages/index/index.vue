<template>
	<!-- 首页 -->
	<view class="index-container">
		<view class="index-swiper">
			<u-swiper :list="list" :effect3d="true" title mode="rect" @click="getNewsById"></u-swiper>
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
			<u-divider :half-width='350' :margin-top="10" :margin-bottom="10">快速直达</u-divider>
			<view class="index-content-buttons">
				<index :menuLists="menuLists" @gotoTargePage="gotoTargePage" />
			</view>
			<u-divider :half-width='350' :margin-top="10" :margin-bottom="10">热点新闻</u-divider>
			<view class="index-content-news">
				
			</view>
		</view>
		<view class="index-draw">
			<u-popup v-model="showDraw" width="300rpx">
				<view>
					<popupItem :menuList="drawMenuList" @gotoPage="gotoPage" />
				</view>
			</u-popup>
		</view>
	</view>
</template>

<script>
import moment from 'moment'
import getWeek from '@/static/js/getWeek.js'
import index from '@/components/buttons/index.vue'
import popupItem from '@/components/popupItem/popupItem.vue'
import tuiDrawer from "@/components/thorui/tui-drawer"
export default {
	components:{
		index,
		popupItem
	},
	data() {
		return {
			text: ['挂号请关注《南华大学附属第一医院公众号》进行操作...'],
			list: [],
			// 15个icon
			icons: [
				'门诊指南',
				'急诊指南',
				'住院指南',
				'挂号预约',
				'诊室分布',
				'医保服务',
				'交通指南',
				'泊车指南',
				'生活指南',
				'医院介绍',
				'患者飞鸿',
				'医院荣誉',
				'健康文章',
				'健康视频'
			],
			menuLists: [],
			content: [],
			
			itemList: [{
				head: "赏识在于角度的转换",
				body: "只要我们",
				disabled: false
			},{
				head: "生活中不是缺少美",
				body: "学会欣赏",
				open: false,
			},{
				head: "周围一些不起眼的人",
				body: "但是据说雕刻好",
				open: false,
			}],
			
			showDraw: false,
			drawMenuList: [],
			date: undefined,
			week: undefined
		}
	},
	onLoad() {
		this.getSwiperImgs()
		this.getNow()
		this.getMenusList()
	},
	methods: {
		change(e) {
			console.log(e);
		},
		itemChange(e) {
			console.log(e);
		},
		getSwiperImgs() {
			const that = this
			uni.request({
				url: 'http://localhost:3000/recruit/getSwiperImgs2Run',
				method: 'GET',
				data: { swiperFlag: 'website' },
				header: {token: 'token'},
				success(res) {
					const { swipers } = res.data
					that.list = swipers.map(e => {
						return { id: e.id, image: e.url, title: e.text, newsid:e.newsid }
					})
				}
			})
		},
		getNow() {
			this.date = moment().format('YYYY年MM月DD日')
			this.week = getWeek(moment().day())
		},
		getNewsById(index) {
			const newsid = this.list[index].newsid
			uni.navigateTo({
				url: `swiper/scanNews/scanNews?newsid=${newsid}`
			})
		},
		openListDraw() {
			this.showDraw = !this.showDraw
		},
		// 获取menus
		getMenusList() {
			const that = this
			uni.request({
				url: 'http://localhost:3000/website/getMenusList',
				method: 'GET',
				success(res) {
					const { items } = res.data
					that.drawMenuList = items
					let temp = {}
					// // 取icons和items的交集
					const Itemstemp = []
					items.forEach(e => {
						e.children.forEach(c => {
							Itemstemp.push(c)
						})
					})
					that.content = Itemstemp.filter(e => {
						if (that.icons.includes(e.name)) {
							return e
						}
					})
					for (const i in that.icons) {
						for (const j in that.content) {
							if (that.content[j].name === that.icons[i]) {
								temp = {
									id: parseInt(i) + 1,
									icon: '../../static/images/icons/'+that.icons[i]+'.png',
									content: that.content[j]
								}
								break
							}
						}
						that.menuLists.push(temp)
						temp = {}
					}
					that.menuLists.push({
						id: 15,
						icon: '../../static/images/icons/服务热线.png',
						content: { name: '服务热线', index: '/jyzn/fwrx' }
					})
				}
			})
		},
		gotoTargePage(obj) {
			// 15个cube跳转到不同的页面
			console.log(obj);
		},
		// 折叠面板subitem跳转到指定页面
		gotoPage(obj) {
			uni.navigateTo({
				url: `targePage/index?category=${obj.name}`
			})
		}
	}
}
</script>
<style>
	.u-list-image-wrap {
		width: 100% !important;
		height: 100% !important;
	}
</style>
<style lang="scss" scoped>
.index-container {
	.index-swiper {
		
	}
	.index-content {
		padding: 20rpx;
		.index-content-header {
			font-size: 25rpx;
			display: flex;
			justify-content: space-between;
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
