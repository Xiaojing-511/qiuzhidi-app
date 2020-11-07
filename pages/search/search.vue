<template>
	<view class="content">
		<view>
			<u-search shape="round" :clearabled="false" :animation="false" :show-action="false" height="64" placeholder="请输入工作或公司"
			 v-model="searchWord" bg-color='#F2F2F2' @click="goSearchDetail" :disabled="true"></u-search>
		</view>
		<view class="tab-box ">
			<u-tabs name="cate_name" count="cate_count" :list="list" :is-scroll="false" :current="current" @change="change"
			 :show-bar='false' font-size="30" class="tabs" active-color='#333333' inactive-color='#999999'></u-tabs>
		</view>
		<view class="comp_list ">
			<u-empty text="搜索内容为空" mode="search" :show='isEmpty'></u-empty>
			<view v-for="(item,index) in searchList.list" :key="item.companyId" class="detail-circulate">
				<ul>
					<li class='detail-title'>
						<text>{{item.recruitName}}</text>
						<span>{{item.recruitSalary}}</span>
					</li>
					<li class='detail-timer'>
						<span>{{item.recruitWorkspace}}</span>
						<span>{{item.releaseDate.slice(0,10)}}</span>
					</li>
					<li class='detail-imgs-box'>
						<image :src="'http://www.qingmengtech.com:8083/'+item.companyLogo" mode="" class="detail-imgs"></image>
						<view class="detail-name">
							<view class="detail-name_text">
								<text>{{item.companyName}}</text>
							</view>
							<view class="detail-name_span">
								<span>{{item.skillRequired}}</span>
							</view>
						</view>
					</li>
				</ul>
			</view>
		</view>
		<u-toast ref="uToast" />
		<u-back-top :scrollTop="scrollTop" :mode="mode" :icon-style="iconStyle" bottom="0"></u-back-top>
	</view>
</template>

<script>
	export default {
		onLoad(option) { //option为object类型，会序列化上个页面传递的参数
			console.log(option.name); //打印出上个页面传递的参数。
			this.searchWord = option.name
			this.search()

		},
		data() {
			return {
				scrollTop: 0,
				mode: 'square',
				iconStyle: {
					fontSize: '32rpx',
					color: '#2979ff'
				},
				searchWord: '',
				list: [{
					cate_name: '实习'
				}, {
					cate_name: '校招'
				}, {
					cate_name: '社招'
				}],
				current: 0,
				isEmpty: false,
				searchList: {
					list: [],
					isFinsh: true,
					totalPage: 1,
					pageNum: 1,
				}
			}
		},
		onReachBottom() {
			this.upData()
		},
		methods: {
			search() {
				this.isEmpty = false
				if (this.searchWord != 0) {
					this.$u.post('http://47.94.151.232:8083/esRecruit/search', {
						keyword: this.searchWord,
						type: this.current
					}).then((res) => {
						console.log(res)
						this.searchList.list = res.data.list
						this.searchList.totalPage = res.data.totalPage
						this.searchList.pageNum = 1
						if (res.data.list.length == 0) {
							this.isEmpty = true
						}
					})
				}
			},
			change(index) {
				this.current = index;
				console.log(this.current);
				this.search()
			},
			goSearchDetail() {
				uni.navigateTo({
					url: '../searchDetail/searchDetail?name=' + this.searchWord
				})
			},
			upData() {
				console.log(this.searchList.pageNum)
				console.log(this.searchList.pageNum != this.searchList.totalPage)

				if (this.searchList.pageNum == this.searchList.totalPage) {
					this.$refs.uToast.show({
						title: '没有更多内容咯',
						type: 'warning',
						position: 'top'
					})
				}
				if (this.searchList.pageNum != this.searchList.totalPage && this.searchList.isFinsh) {
					this.searchList.isFinsh = false
					this.searchList.pageNum++;
					this.$u.post('http://47.94.151.232:8083/esRecruit/search', {
						keyword: this.searchWord,
						type: this.current,
						pageNum: this.searchList.pageNum
					}).then((res) => {
						this.searchList.isFinsh = true
						this.searchList.list = this.searchList.list.concat(res.data.list)
						console.log(this.data.searchList.list)
						console.log(res)

					})
				}
			}
		}
	}
</script>

<style>
	.content {
		width: 100%;
		padding: 0 30rpx;
		
	}

	.detail-title {
		display: flex;
		justify-content: space-between;
	}

	.detail-timer {
		display: flex;
		justify-content: space-between;
		font-weight: 100;
		font-size: 20rpx;
	}

	.detail-imgs {
		width: 80rpx;
		height: 80rpx;
	}

	.detail-imgs-box {
		display: flex;

	}

	.detail-name {
		margin-left: 50rpx;
	}

	.detail-circulate {
		background-color: #FFFFFF;
		margin-left: -30rpx;
		margin-right: -30rpx;
		padding: 0 30rpx;
		border-bottom: 2rpx solid #C8C9CC;

	}

	.detail-title span {
		margin: auto 0rpx;
		color: #FA3534;
	}

	.detail-title {
		margin: 20rpx 0;
	}

	.detail-timer {
		margin-bottom: 30rpx;
	}

	.detail-title text {
		font-size: 40rpx;
		font-weight: 700;

	}

	.detail-name {
		margin-bottom: 40rpx;
	}

	.enmpty {

		margin: 300rpx auto;
	}

	.tabs {
		font-family: STSong;
		font-style: normal;
		font-weight: normal;
		font-size: 30rpx;
		line-height: 30rpx;
		/* identical to box height */

		text-align: center;
		letter-spacing: -0.333333px;

		color: #000000;
	}
</style>
