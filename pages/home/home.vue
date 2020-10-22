<template>
	<view class="content">
		<u-swiper :list="imgs"></u-swiper>
		<view class="dtlbox">
			<view>
				<image src="../../static/img/switch_identity_2.png" class="dtl-img"></image>
				<view class="dtl-word">内推</view>
			</view>
			<view>
				<image src="../../static/img/switch_identity_2.png" class="dtl-img"></image>
				<view class="dtl-word">面试指导</view>
			</view>
			<view>
				<image src="../../static/img/switch_identity_2.png" class="dtl-img"></image>
				<view class="dtl-word">我要招人</view>
			</view>
		</view>
		<view class="srchbox u-m-t-20">
			<input type="text" class="input-search" placeholder="请输入" v-model="seachText" />
			<u-button :custom-style="customStyle" shape="circle">搜索</u-button>
		</view>
		<view class="search-bar" v-for="(item,index) in showList" :key='index'>
			
		</view>
		<view class="tab-box">
			<u-tabs name="cate_name" count="cate_count" :list="list" :is-scroll="false" :current="current" @change="change"
			 font-size="30" class="tabs"></u-tabs>
		</view>

		<view class="">
			<view v-for="(item,index) in dataList" :key="item.companyId">
				<ul>
					<li class='detail-title'>
						<h1>{{item.recruitName}}</h1>
						<span>{{item.recruitSalary}}</span>
					</li>
					<li class='detail-timer'>
						<span>{{item.recruitWorkspace}}</span>
						<span>{{item.releaseDate}}</span>
					</li>
					<li class='detail-imgs-box'>
						<image src="../../static/img/switch_identity_1.png" mode="" class="detail-imgs"></image>
						<view class="detail-name">
							<h3>公司</h3>
							<span>D轮级以上/1000-9999人/互联网</span>
						</view>

					</li>
				</ul>
			</view>

		</view>
	</view>
</template>

<script>
	export default {
		onLoad() {
			this.getimgs()
			this.changeDatalist()
		},
		data() {
			return {
				list: [{
					cate_name: '实习'
				}, {
					cate_name: '校招'
				}, {
					cate_name: '社招'
				}],

				current: 0,
				imgs: [],
				seachText: '',
				searchList: [],
				showList: [],
				dataList: [],
				customStyle: {
					width: '120rpx',

				},
			}
		},
		methods: {
			change(index) {
				this.current = index;
				console.log(this.current);
				this.changeDatalist()
			},
			changeDatalist() {
				this.$u.post('/recruit/information', {
					type: this.current
				}).then((res) => {
					console.log(res)
					this.dataList = res.list
					console.log(this.dataList)
				})
			},
			getimgs() {
				this.$u.get('/recruit/rotation').then((res) => {
					this.imgs = res.map((value) => {
						return 'http://118.24.96.51:8085/' + value.url
					})
					console.log(this.imgs)
				})
			},
			
		}
	}
</script>

<style lang="scss">
	.content {
		width: 100%;
		margin-left: 20rpx;
		margin-right: 20rpx;
	}

	,
	.dtlbox {
		display: flex;
		-webkit-justify-content: space-around;
		justify-content: space-around;
	}

	.dtl-img {
		height: 34px;
		width: 38px;

	}

	.dtl-word {
		font-family: STSong;
		font-size: 30rpx;
		font-style: normal;
		font-weight: 400;
		line-height: 30rpx;
		letter-spacing: -0.3333333432674408px;
		text-align: center;

	}

	.srchbox {
		display: flex;
		justify-content: space-between;
	}

	.search-bar {
		position: absolute;
		z-index: 9;
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

	.tab-box {
		width: 50%;

	}

	.input-search {
		width: 500rpx;
		height: 80rpx;
		border-radius: 4px;
		background-color: #C4C4C4;

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
</style>
