<template>
	<view class="content">
		<view>
			<u-search shape="round" :clearabled="false" :animation="false" :show-action="false" height="64" placeholder="请输入工作或公司"
			 v-model="searchWord" bg-color='#F2F2F2' @search="search"></u-search>
		</view>
		<view class="tab-box ">
			<u-tabs name="cate_name" count="cate_count" :list="list" :is-scroll="false" :current="current" @change="change"
			 :show-bar='false' font-size="30" class="tabs" active-color='#333333' inactive-color='#999999'></u-tabs>
		</view>
		<view class="comp_list ">
			<view v-for="(item,index) in innerList.list" :key="item.companyId" @click="goDetail" class="detail-circulate" v-show="current==0">
				<ul>
					<li class='detail-title'>
						<text>{{item.recruitName}}</text>
						<span>{{item.recruitSalary}}</span>
					</li>
					<li class='detail-timer'>
						<span>{{item.recruitWorkspace}}</span>
						<span>{{item.releaseDate}}</span>
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
			<view v-for="(item,index) in shcoolList.list" :key="item.companyId" @click="goDetail" class="detail-circulate"
			 v-show="current==1">
				<ul>
					<li class='detail-title'>
						<text>{{item.recruitName}}</text>
						<span>{{item.recruitSalary}}</span>
					</li>
					<li class='detail-timer'>
						<span>{{item.recruitWorkspace}}</span>
						<span>{{item.releaseDate}}</span>
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
			<view v-for="(item,index) in socialList.list" :key="item.companyId" @click="goDetail" class="detail-circulate"
			 v-show="current==2">
				<ul>
					<li class='detail-title'>
						<text>{{item.recruitName}}</text>
						<span>{{item.recruitSalary}}</span>
					</li>
					<li class='detail-timer'>
						<span>{{item.recruitWorkspace}}</span>
						<span>{{item.releaseDate}}</span>
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
	</view>
</template>

<script>
	export default {
		data() {
			return {
				searchWord: '',
				list: [{
					cate_name: '实习'
				}, {
					cate_name: '校招'
				}, {
					cate_name: '社招'
				}],
				current: 0,
				searchList: {
					list: [],
					isFinsh: true,
					totalPage: 1,
					pageNum: 1,
				},
				innerList: {
					list: [],
					isFinsh: true,

				},
				shcoolList: {
					list: [],
					isFinsh: true,

				},
				socialList: {
					list: [],
					isFinsh: true,

				},
			}
		},
		onReachBottom() {
			this.upData()
		},
		methods: {
			search() {
				if (this.searchWord != 0) {
					this.$u.post('http://47.94.151.232:8083/esRecruit/search/simple', {
						keyword: this.searchWord
					}).then((res) => {
						console.log(res)
						this.searchList.list = res.list
						this.searchList.totalPage = res.totalPage
						this.searchList.pageNum = 1
						this.getTab()
					})
				}
			},
			change(index) {
				this.current = index;
				console.log(this.current);
				this.getTab()
			},
			getTab() {
				if (this.current == 0) {
					this.innerList.list = this.searchList.list.filter((value) => {
						if (value.type == 0) {
							return value
						}
					})
					if (this.innerList.list.length < 4) {
						this.upData()
					}
				} else if (this.current == 1) {
					this.shcoolList.list = this.searchList.list.filter((value) => {
						if (value.type == 1) {
							return value
						}
					})
					if (this.shcoolList.list.length < 4) {
						this.upData()
					}
				} else if (this.current == 2) {
					this.socialList.list = this.searchList.list.filter((value) => {
						if (value.type == 2) {
							return value
						}
					})
					if (this.socialList.list.length < 4) {
						this.upData()
					}
				}
				console.log(this.innerList.list)
				console.log(this.shcoolList.list)
				console.log(this.socialList.list)

			},
			upData() {
				console.log(this.searchList.pageNum)
				console.log(this.searchList.pageNum != this.searchList.totalPage)
				if (this.searchList.pageNum != this.searchList.totalPage && this.searchList.isFinsh) {
					this.searchList.isFinsh = false
					this.searchList.pageNum++;
					this.$u.post('http://47.94.151.232:8083/esRecruit/search/simple', {
						keyword: this.searchWord,
						pageNum: this.searchList.pageNum
					}).then((res) => {
						this.searchList.isFinsh = true
						this.searchList.list = this.searchList.list.concat(res.list)
						console.log(this.searchList.list)
						console.log(res)
						this.getTab()
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
