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
			<view class="index-content-dropdown">
				<newsdropdown />
			</view>
			<view class="index-content-news">
				<newsBox v-for="item in newsList" :key="item.id" :newsItem="item" @getCardNewsById="getCardNewsById" />
				<text class="loading-text">
				{{loadingType === 0 ? contentText.contentdown : (loadingType === 1 ? contentText.contentrefresh : contentText.contentnomore)}}
				</text>
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
import news from '@/components/news/news.vue'
import index from '@/components/buttons/index.vue'
<<<<<<< HEAD
=======
<<<<<<< HEAD

export default {
	components:{
		index,
		news
=======
>>>>>>> 027874b98899deb277d419feddb982fcdd94c32f
import popupItem from '@/components/popupItem/popupItem.vue'
import tuiDrawer from "@/components/thorui/tui-drawer"
import newsBox from "@/components/news/newsBox/index.vue"
import newsdropdown from '@/components/news/dropdown/index.vue'
export default {
	components:{
		index,
<<<<<<< HEAD
		popupItem,
		newsBox,
		newsdropdown
=======
		popupItem
=======
		index
>>>>>>> f0d9e553d1004b3533e4033047b71c4ff9909493
>>>>>>> e59107ab24bb084c9c4e5bcf5c73ee5e59f86299
>>>>>>> 9c9030bc06eb848c9bc5e7341df51c16916aca47
>>>>>>> 027874b98899deb277d419feddb982fcdd94c32f
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
			
			showDraw: false,
			drawMenuList: [],
			date: undefined,
			week: undefined,
			// 加载更多的相关变量
			contentText: {
				pullRefreshStatus:false,//下拉刷新
				contentdown: "上拉显示更多",
				contentrefresh: "正在加载...",
				contentnomore: "没有更多数据了"
			},
			loadingType: 0,
			newsList: [],
			// 每页条数
			limit: 5,
			// 总页数
			page: 1,
			// 总条数
			total: 0,
			// 总页数
			countPage: 0
		}
	},
	onLoad() {
		this.getSwiperImgs()
		this.getNow()
		this.getMenusList()
		this.getNewsList()
	},
	onReachBottom() {
		
		setTimeout(() => {
			const that = this
			that.page++
			if (that.loadingType !== 0) {
				return false
			}
			that.loadingType = 1
			uni.showNavigationBarLoading()
			uni.request({
				url: 'http://localhost:3000/wechat/news/getnewsList',
				data: { limit: that.limit, page: that.page, role: 'wechat' },
				header: { token: 'token' },
				success(res) {
					const { items, total } = res.data
					that.total = total
					let tempCountPage
					that.newsList = [...that.newsList, ...items]
					// /没有数据
					if (that.newsList.length === total) {
						that.loadingType = 2
						uni.hideNavigationBarLoading()
						return
					}
					that.loadingType = 0
					uni.hideNavigationBarLoading()
				}
			})
		},2000)
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
		// 点击swiper跳转到新闻浏览页面
		getNewsById(index) {
			const newsid = this.list[index].newsid
			uni.navigateTo({
				url: `swiper/scanNews/scanNews?newsid=${newsid}`
			})
		},
		// 新闻卡片跳转到新闻浏览页面
		getCardNewsById({id}) {
			uni.navigateTo({
				url: `swiper/scanNews/scanNews?newsid=${id}`
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
		},
		// 获取新闻列表
		getNewsList() {
			const that = this
			uni.request({
				url: 'http://localhost:3000/wechat/news/getnewsList',
				data: { limit: that.limit, page: that.page, role: 'wechat' },
				header: { token: 'token' },
				success(res) {
					const { items, total } = res.data
					that.total = total
					that.newsList = items
				}
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
		.index-content-dropdown {
			// display: flex;
			// justify-content: flex-start;
		}
		.index-content-news {
			.loading-text{
					height: 80upx;
					line-height: 80upx;
					font-size: 26upx;
					color:#ccc;
					display: flex;
					flex-direction: row;
					justify-content: space-around;
				}
		}
	}
}
</style>