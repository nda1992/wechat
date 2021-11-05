<template>
	<view class="data-container">
		<!-- 昨日基本数据 -->
		<view class="data-basic">
			<view class="data-basic-header">{{ yesterday }}基本数据</view>
			<view class="data-basic-show">
				<basicData v-for="(item, index) in showData " :key='index' :showData="item" />
			</view>
		</view>
		
		<u-sticky :offset-top="375">
			<view class="data-tabs">
				<u-tabs :list="list" :is-scroll="false" :current="current" @change="change"></u-tabs>
				<view class="data-tabs-first" v-if="current===0">
					<view @click="openSelect">请选择要查询的类型<text style="color: red;">(必选)</text></view>
					<view style="margin: 25rpx 0;">
						<text v-if="select_name!==''">您已选择:<text style="color: #2979FF;">{{ select_name }}</text></text>
					</view>
					<view @click="openCalendar">请选择要查询的日期<text style="color: red;">(必选)</text>
					<view style="margin: 25rpx 0;">
						<text v-if="select_date.startDate!=='' && select_date.endDate !== ''">
							您已选择:<text style="color: #2979FF;">{{ select_date.startDate }}~{{ select_date.endDate }}</text>
						</text>
					</view>
					</view>
					<view style="margin: 30rpx 15rpx;">
						<u-button :ripple="true" ripple-bg-color="#F1F1F1" type="primary" @click="getRevenue">查询</u-button>
						</view>
					<view class="data-tabs-first-show">
						<u-divider v-if="select_data.length===0">暂无数据</u-divider>
						<selectData :showSelectData='select_data' :tip='tip' v-else></selectData>
					</view>
					
				</view>
				<view class="data-tabs-second" v-if="current===1"></view>
			</view>
		</u-sticky>
		
		<u-select v-model="show_select" :list="select_list" @confirm='select_mode'></u-select>
		<u-calendar v-model="show_calendar" :mode="'range'" @change="calendar_change"></u-calendar>
		<u-top-tips ref="uTips"></u-top-tips>
	</view>
</template>

<script>
import moment from 'moment'	
import basicData from '@/components/basicData.vue'
import selectData from '@/components/selectData.vue'
export default {
	components: {
		basicData,
		selectData
	},
	data() {
		return {
			yesterday: undefined,
			loading: false,
			list: [
				{name: '基本数据'},
				{name: '数据可视化'}
			],
			current: 0,
			// 展示昨日的部分数据
			showData: [],
			// 列选择器
			show_select: false,
			select_list: [{value: '1',label: '门诊'}, {value: '2', label: '住院'}, {value: '3', label: '全院'}],
			select_name: '',
			
			// 日历
			select_date: { startDate: '', endDate: '' },
			show_calendar: false,
			select_calendar: '',
			
			// 未选择必须参数时的错误提示
			showTips: false,
			
			// 最终显示的数据
			select_data: [],
			tip: ''
		}
	},
	onLoad() {
		this.getBaiscData()
		this.yesterday = moment().subtract(1, 'days').format('YYYY年MM月DD日')
	},
	methods: {
		change(index) {
			this.current = index
		},
		// 获取基本数据
		getBaiscData() {
			const that = this
			uni.request({
				url: 'http://localhost:3000/wechat/getDailyData',
				method: 'GET',
				header: { token: 'token' },
				success(res) {
					const { result } = res.data
					const obj = {
						'门急诊人次': result.门急诊人次,
						'入院人数': result.入院人数,
						'出院人数': result.出院人数,
						'门急诊收入': result.门急诊收入,
						'住院收入': result.住院收入,
						'全院收入': result.全院收入,
						'药占比': result.药占比,
						'耗占比': result.耗占比,
						'在院人数': result.在院人数
					}
					const values = Object.values(obj)
					const keys = Object.keys(obj)
					const result_arr = []
					for (let i in values) {
						result_arr.push({ 'name': keys[i], 'value': values[i] })
					}
					that.showData = result_arr
				}
			})
		},
		// 打开选择器
		openSelect() {
			this.show_select = true
		},
		// 列选择器触发
		select_mode(e) {
			this.select_name = e[0].label
		},
		// 打开日历
		openCalendar() {
			this.show_calendar = true
		},
		calendar_change(e) {
			this.select_date.startDate = e.startDate
			this.select_date.endDate = e.endDate
		},
		getRevenue() {
			if (this.select_date.startDate === '' || this.select_date.endDate === '' && this.select_name === '') {
				this.$refs.uTips.show({
					title: '请选择必须的参数',
					type: 'error',
					duration: '3000'
				})
				return
			}
			const temp = Object.assign({}, this.select_date, { type: this.select_name })
			const that = this
			uni.request({
				url: 'http://localhost:3000/wechat/getRevenue',
				method: 'POST',
				data: temp,
				header: { token: 'token' },
				success(res) {
					if (res.data.code === 200) {
						that.$refs.uTips.show({
							title: '数据获取成功',
							type: 'success',
							duration: '2000'
						})
						const { tip, items } = res.data
						that.tip = tip
						const keys = Object.keys(items[0])
						const values = Object.values(items[0])
						if (values[0]===null) {
							that.$refs.uTips.show({
								title: '暂无数据',
								type: 'warning',
								duration: '2000'
							})
							return
						}
						const result_arr = []
						for (let i in keys) {
							result_arr.push({ 'name': keys[i], 'value': values[i] })
						}
						that.select_data = result_arr
					}
				}
			})
		}
	}
}
</script>

<style lang="scss" scoped>
	.data-container {
		.data-basic {
			background-color: #F8F8F8;
			box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
			padding: 10rpx;
			width: 100%;
			height: 380rpx;
			border-radius: 15rpx;
			.data-basic-header {
				display: flex;
				justify-content: center;
				align-items: center;
			}
			.data-basic-show {
				display: flex;
				flex-wrap: wrap;
				justify-content: space-around;
			}
		}
		.data-tabs {
			.data-tabs-first {
				padding: 0 15rpx;
				margin-top: 30rpx;
			}
		}
	}
</style>
