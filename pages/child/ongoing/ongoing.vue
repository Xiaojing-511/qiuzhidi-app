<template>
	<scroll-view scroll-y class="album_scroll" @scrolltolower="handleToLower">
		<view class="main">
			<view class="message_list" v-for="item in goods" :key="item.id">
				<view class="message">
					<text class="recruit_name">{{item.recruitName}}</text>
					<text class="recruit_salary">{{item.recruitSalary}}</text>
					<view class="place">
						<text class="place-item">{{item.recruitWorkspace}}</text>
						<text class="type">{{['实习','校招','社招'][Number(item.type)]}}</text>
						<text class="time">{{item.releaseDate.substring(0,10)}}</text>
					</view>
				</view>
				<view class="main-item">
					<image class="identity-img"></image>
					<view class="company">{{item.department}}</view>
					<view class="jobDescription">{{item.jobDescription}}</view>

				</view>
				<view class="status">
					<text v-if="item.isClose=='1'">{{closed}}</text>
					<text v-else>{{going}}</text>
					<button @click="rewriteClick(item.id)" v-if="item.isClose=='1'" class="rewrite">
						{{rewrite}}
					</button>
					<button @click="closeClick(item.id)" type="default" v-else class="close">
						{{close}}
					</button>
				</view>
			</view>
		</view>
	</scroll-view>
</template>

<script>
	export default {

		data() {
			return {
				goods: [],
				rewrite: '重发',
				close: '关闭',
				closed: '已关闭',
				going: '进行中',
				params: {
					pageSize: 20,
					pageNum: 1
				},
				total: 0,
				totalPage: 0,
				hasmore: true
			}
		},
		mounted() {
			this.getAllList()
		},
		onReachBottom() {
			console.log('到底啦')
		},

		methods: {
			getAllList() {
				this.$u.post('recruit/getRecruitList', {
					isClose: 0,
					pageSize: this.params.pageSize,
					pageNum: this.params.pageNum
				}).then(res => {
					console.log(res)
					this.totalPage = (res.data.total + this.params.pageSize - 1) / this.params.pageSize;
					if (this.params.pageNum >= this.totalPage) {
						this.hasmore = false
						return
					}

					this.goods = [...this.goods, ...res.data.list]
					this.setData({
						params: this.data.params
					})
					// 将token存储在vuex中（用的uview优化过的写法，vuex_token字段配置了，可以直接存在了本地存储中）
					// this.$u.vuex('vuex_token', res.token)
					// 返回原本所在页面
					// uni.navigateBack()
				}).catch(res => {
					// this.showToast(res.message, 'error')
				})
			},
			closeClick(id) {
				this.$u.post('recruit/closeRecruitByRecruitId', {
					recruitId: id
				}).then(res => {
					this.$u.post('recruit/getRecruitList', {
						isClose: 0,
						pageSize: this.params.pageSize,
						pageNum: this.params.pageNum
					}).then(res => {
						this.goods = res.data.list
					})
					console.log(id)
					uni.showToast({
						icon: 'none',
						title: '关闭成功'
					})
					// 将token存储在vuex中（用的uview优化过的写法，vuex_token字段配置了，可以直接存在了本地存储中）
					// this.$u.vuex('vuex_token', res.token)
					// 返回原本所在页面
					// uni.navigateBack()
				}).catch(res => {
					// this.showToast(res.message, 'error')
				})
			},
			handleToLower() {
				if (this.hasmore) {
					this.params.pageNum += 1
					this.getAllList()
				} else {
					uni.showToast({
						title: "没有数据了",
						icon: "none"
					})
				}

			}

		}
	}
</script>

<style lang="scss">
	.album_scroll {
		height: calc(100vh - 37px);
	}

	.main {
		height: 320rpx;
		background-color: #FFFFFF;

		.message_list {
			height: 320rpx;
			margin-top: 30rpx;
			margin-bottom: 30rpx;
			background-color: #FFFFFF;
			width: 100%;

			.message {
				padding: 24rpx;


				.recruit_name {

					font-family: PingFangSC;
					font-weight: 600;
					font-size: 36rpx;
					color: #333333;

				}

				.recruit_salary {
					float: right;
					padding-top: 10rpx;
					font-size: 30rpx;
					font-family: PingFangSC;
					font-weight: 400;
					color: #FF0000;

				}

				.time {
					float: right;
					font-size: 18rpx;
					font-family: PingFangSC;
					font-weight: 400;
					color: #999999;

				}

				.place {
					font-size: 20rpx;
					color: #999999;
					margin-top: 16rpx;

					.place-item {
						padding: 4rpx 10rpx;
						margin-right: 10rpx;
						background-color: #EEEEEE;
						border-radius: 6rpx;
					}

					.type {
						padding: 4rpx 10rpx;
						color: #999999;
						background-color: #EEEEEE;
						border-radius: 6rpx;
					}
				}
			}
		}

		.main-item {
			display: flex;
			padding-bottom: 10rpx;
			border-bottom: 2rpx solid #DDDDDD;
			position: relative;

			.identity-img {
				width: 100rpx;
				height: 100rpx;
				border-radius: 14rpx;

			}

			.company {
				font-size: 28rpx;
				font-family: PingFangSC;
				font-weight: 400;
				color: #333333;
				margin-right: 30rpx
			}

			.jobDescription {
				font-size: 24rpx;
				font-family: PingFangSC;
				font-weight: 400;
				color: #333333;
				position: absolute;
				top: 40rpx;
				left: 80rpx;
			}
		}



		.status {
			display: flex;
			padding-top: 10rpx;
			height: 72rpx;

			// line-height: 72rpx;
			text {
				padding-top: 0;
				padding-left: 16rpx;
			}

			.close {
				width: 110rpx;
				height: 48rpx;
				background-color: #00A8FF;
				font-size: 20rpx;
				float: right;
				margin-right: 20rpx;
				color: #FFFFFF;
				font-family: PingFangSC;
				font-weight: 400;
				border-radius: 6rpx;
			}

		}

	}
</style>
