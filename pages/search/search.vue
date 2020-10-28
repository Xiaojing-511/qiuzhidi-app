<template>
	<view class="content">
		<view>
			<u-search shape="round" :clearabled="false" :animation="false" :show-action="false" height="64" placeholder="请输入工作或公司"
			 v-model="searchWord" bg-color='#F2F2F2' @search="search"></u-search>
		</view>
		<view class="comp_list ">
			<view v-for="(item,index) in searchList.list" :key="item.companyId" @click="goDetail" class="detail-circulate">
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
	</view>
</template>

<script>
	export default {
		data() {
			return {
				searchWord: '',
				searchList: {
					list: [],
					isFinsh: true,
					totalPage: 1,
					pageNum: 1,
				}
			}
		},
		onReachBottom(){
			this.upData()
		},
		methods: {
			search() {
				if (this.searchWord != 0) {
					this.$u.post('http://118.24.96.51:8083/esRecruit/search/simple', {
						keyword: this.searchWord
					}).then((res) => {
						console.log(res)
						this.searchList.list = res.list
						this.searchList.totalPage=res.totalPage
						this.searchList.pageNum= 1
					})
				}
			},
			upData(){
				
				console.log(this.searchList.pageNum)
				console.log(this.searchList.pageNum!=this.searchList.totalPage)
				if (this.searchList.pageNum!=this.searchList.totalPage&&this.searchList.isFinsh) {
					this.searchList.isFinsh=false
					this.searchList.pageNum++;
					this.$u.post('http://118.24.96.51:8083/esRecruit/search/simple', {
						keyword: this.searchWord,
						pageNum:this.searchList.pageNum
					}).then((res) => {
						this.searchList.isFinsh=true
						this.searchList.list = this.searchList.list.concat(res.list)
						console.log(this.searchList.list)
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
</style>
