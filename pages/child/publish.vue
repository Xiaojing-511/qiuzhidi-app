<template>
	<view class="body">
		<!-- 顶部选项卡 -->
		<view class="header">
			<view v-for="(tab,index) in tabBars" :key="tab.id" :class="{btna:count == index}" @tap="change(index)">
				<text>{{tab.name}}</text>
				<text></text>
			</view>
		</view>

		<view class="" v-if="hackreset">
			<view class="end" :class="{dis:btnnum == 0}">
				<All></All>
			</view>
			<view class="end" :class="{dis:btnnum == 1}">
				<on-going></on-going>
			</view>
			<view class="end" :class="{dis:btnnum == 2}">
				<closed></closed>
			</view>
		</view>
		<view class="btn-row">
			<button @click="btn">发一条</button>
		</view>
		<!-- 底部导航 -->
		<view class="tab">
			<view class="tab-item" v-for="(item,index) in navs" :key="index" @click="tabItemClick(item.path)">
				<view>{{item.title}}</view>
			</view>

		</view>

	</view>
</template>

<script>
	import All from "./all/all.vue"
	import onGoing from "./ongoing/ongoing.vue"
	import closed from "./closed/closed.vue"

	export default {
		components: {
			All,
			onGoing,
			closed

		},
		data() {
			return {
				tabBars: [{
						name: '全部',
						id: 'all'
					},
					{
						name: '进行中',
						id: 'on-going'
					}, {
						name: '已关闭',
						id: 'closed'
					}
				],
				hackreset: true,
				btnnum: 0,
				count: "",
				goods: [],
				params: {
					pageNum: 1,
					pageSize: 20
				},
				navs: [{
					title: '首页',
					path: '/pages/hello/hello'
				}, {
					title: '投递',
					path: '/pages/send/send'
				}, {
					title: '我的',
					path: '/pages/my/my'
				}]
			}
		},
		onLoad() {

		},
		methods: {
			btn() {
				uni.navigateTo({
					url: '../information/information',

				})
			},
			tabItemClick(url) {
				console.log(url)
				uni.navigateTo({
					url
				})
			},
			change(e) {
				this.count = e
				this.btnnum = e
				this.hackreset = false
				if (e == 0) {
					console.log(this.btnnum)
					this.$u.post('recruit/getRecruitList', {
						isClose: -1,
						pageSize: this.params.pageSize,
						pageNum: this.params.pageNum
					}).then(res => {
						console.log(res)
						this.goods = res.data.list
						this.$forceUpdate()
					})
				} else if (e == 1) {
					this.$u.post('recruit/getRecruitList', {
						isClose: 0,
						pageSize: this.params.pageSize,
						pageNum: this.params.pageNum
					}).then(res => {
						console.log(res)
						this.goods = res.data.list
						this.$forceUpdate()
					})
				} else {
					this.$u.post('recruit/getRecruitList', {
						isClose: 1,
						pageSize: this.params.pageSize,
						pageNum: this.params.pageNum
					}).then(res => {
						console.log(res)
						this.goods = res.data.list
						this.$forceUpdate()
					})
				}
				this.$nextTick(function() {
					this.hackreset = true
				})
			}


		}

	}
</script>

<style lang="scss">
	page {
		background-color: #EEEEEE;
	}

	.body {
		width: 100%;
	}

	.header {
		display: flex;
		padding-top: 30rpx;
		border-top: 2rpx soild #EEEEEE;
		color: #999999;
		padding-left: 0;
		padding-right: 0;
		background-color: #FFFFFF;
		width: 100%;
		justify-content: space-around;


		.btna {
			color: #333333;
			padding-bottom: 30rpx;
			width: 150rpx;
			text-align: center;
			border-bottom: 4rpx solid #00A8FF;

		}
	}



	.end {
		display: none;
		margin-bottom: 110rpx;
	}

	.dis {
		display: block;

	}


	.btn-row {
		position: fixed;
		margin: 0 auto;
		left: 0;
		right: 0;
		bottom: 120rpx;
		font-size: 32rpx;

		button {
			background-color: #00A8FF;
			font-size: 30rpx;
			font-family: PingFangSC;
			font-weight: 400;
			color: #FFFFFF;
			width: 600rpx;
			height: 80rpx;
			line-height: 80rpx;


		}
	}

	.tab {
		display: flex;

		position: fixed;
		bottom: 0;
		width: 100%;
		height: 100rpx;
		background-color: #FFFFFF;
		margin-top: 29rpx;
		font-size: 30rpx;
		line-height: 100rpx;

		justify-content: space-between;

		.tab-item {
			color: #999999;
			padding: 0 80rpx
		}
	}
</style>
