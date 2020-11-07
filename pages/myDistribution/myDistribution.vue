<template>
	<view class="content">
		<view class="tab-box">
			<u-tabs :list="list" :is-scroll="false" :current="current" @change="change" active-color="#333333" inactive-color='#999999' :show-bar="false"></u-tabs>
		</view>
		<view>
			<view v-show="current === 0">
				<ul v-for="(item,index) in interview.list" :key="item.id" class='detail-block'>
					<li class='detail-title'>
						<text class="detail-position">{{item.interviewPosition}}</text>
						<text class="detail-salary">{{item.interviewPrice}}/次</text>
					</li>
					<li class='detail-imgs-box'>
						<image :src="'http://118.24.96.51:8085/'+item.icon" mode="" class="detail-imgs"></image>
						<view class="detail-name">
							<view class="detail-name_text">
								<span class="detial-username">{{item.username}}</span>
								<text class="detial-companyName">{{item.companyName}}</text>
							</view>
							<view class="detail-name_span">
								<span>侧重点：{{item.focus}}</span>
							</view>

						</view>
					</li>
					<view class="certified">已认证</view>
				</ul>

			</view>
			<view v-show="current === 1">
				<ul v-for="(item,index) in recruit.list" :key="item.id" class='detail-block'>
					<li class='detail-title recurit-title'>
						<text class="detail-position">{{item.recruitName}}</text>
						<span class='recurit-salary'>{{item.recruitSalary}}</span>
					</li>
					<li class='detail-timer'>
						<span class='recuitWork'>{{item.recruitWorkspace}}</span>
						<span class='recurit-data'>{{item.releaseDate.slice(0,10)}}</span>
					</li>
					<li class='recurit-imgs-box'>
						<image :src="'http://118.24.96.51:8085/'+item.companyLogo" mode="" class="recurit-imgs"></image>
						<view class="detail-name">
							<view class="detail-name_text">
								<text>{{item.companyName}}</text>
							</view>
							<view class=" rucurit-skill">
								<span>{{item.skillRequired}}</span>
							</view>

						</view>
					</li>
				</ul>
			</view>
		</view>
		<button class="btn" @click="postAgain">再发一条</button>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				list: [{
					name: '面试指导'
				}, {
					name: '招聘信息'
				}],
				current: 0,
				interview: {
					isFinsh: true,
					isLast: false,
					isFirst: true,
					pageNum: 1,
					list: []
				},
				recruit: {
					isFinsh: true,
					isLast: false,
					isFirst: true,
					pageNum: 1,
					list: []
				}
			}
		},
		onLoad() {
			this.firstGet()
		},
		onReachBottom() {
			this.upData()
		},
		methods: {
			change(index) {
				this.current = index;
				this.firstGet()
			},
			firstGet() {
				if (this.current == 0 && this.interview.isFirst) {
					this.getInterview()
					this.interview.isFirst = false
				} else if (this.current == 1 && this.recruit.isFirst) {
					this.getRecruit()
					this.recruit.isFirst = false
				}
			},
			upData() {
				if (this.current == 0 && !this.interview.isLast && this.interview.isFinsh) {
					this.getInterview()
				} else if (this.current == 1 && !this.recruit.isLast && this.recruit.isFinsh) {
					this.getRecruit()
					console.log('11')
				}
			},
			getInterview() {
				if (this.interview.isFinsh) {
					this.interview.isFinsh = false
					this.$u.get('/interviewer/guidance', {
						type: this.current + 1,
						pageNum: this.interview.pageNum
					}).then((res) => {
						console.log(res)
						this.interview.isFinsh = true;
						this.interview.pageNum++;
						this.interview.list = this.interview.list.concat(res.data.list);
						this.interview.isLast = res.data.isLastPage
						console.log(this.interview.list)
					})
				}

			},
			getRecruit() {
				if (this.recruit.isFinsh) {
					this.recruit.isFinsh = false;
					this.$u.get('/interviewer/guidance', {
						type:this.current+1,
						pageNum: this.recruit.pageNum,
						pageSize:5
					}).then((res) => {
						console.log(res)
						this.recruit.isFinsh = true;
						this.recruit.pageNum++;
						this.recruit.list = this.recruit.list.concat(res.data.list);
						this.recruit.isLast = res.data.isLastPage
						console.log(res)
						console.log(this.recruit.list)
					})
				}
			},
			postAgain() {
				if (this.current == 0) {

				}
				if (this.current == 1) {

				}
			}
		}
	}
</script>

<style>
	.detail-salary {
		position: absolute;
		right: 40rpx;
	}

	.detail-title {
		position: relative;
	}
	.recuitWork{
		font-size: 20rpx;
		color: #999999;
	}
	.recurit-salary{
		position: absolute;
		right: 0;
		font-size: 30rpx;
		color: #FF0000;
	}
	.content {
		width: 100%;
		background-color: #EEEEEEEE;
		/* margin:0 30rpx; */
	}
	
	.recurit-imgs{
		height: 100rpx;
		width: 100rpx;
	}
	.btn {

		position: fixed;
		width: 599rpx;
		height: 90rpx;
		margin-left: 76rpx;
		bottom: 139rpx;
		background-color: #00A8FF;
		color: #FFFFFF;
		border-radius: 5rpx;
		z-index: 999;

	}
	.recurit-imgs-box{
		display: flex;
		margin-top: 20rpx;
	}
	.rucurit-skill{
		font-size: 30rpx;
		color: #333333;
		margin-top: 20rpx;
	}
	.certified {
		position: absolute;
		top: 30rpx;
		right: -50rpx;
		transform: rotate(45deg);
		background-color: #00A8FF;
		color: #FFFFFF;
		font-size: 24rpx;
		width: 180rpx;
		text-align: center;
	}

	.recurit-data{
		position: absolute;
		right: 0;
		font-size: 20rpx;
		color: #999999;
	}

	.detail-block {
		overflow: hidden;
		position: relative;
		margin-top: 40rpx;
		background-color: #FFFFFF;
		height: 300rpx;
		padding: 30rpx 30rpx;
		height: 270rpx;

	}
	.detail-timer{
		position: relative;
	}
	.detail-imgs {
		width: 120rpx;
		height: 120rpx;
	}

	.detail-position {
		font-size: 36rpx;
		color: #333333;
		font-weight: 600;
	}

	.detail-imgs-box {
		display: flex;
		margin-top: 30rpx;
	}

	.tab-box {
		width: ;
	}

	.detial-username {
		font-size: 30rpx;
		font-weight: 600;
	}
	.detail-name{
		margin-left: 30rpx;
	}
	.detail-name_span {
		margin-top: 20rpx;
		font-size: 24rpx;
		font-weight: 400;
	}

	.detial-companyName {
		font-size: 24rpx;
		color: #999999;
		margin-left: 20rpx;
	}
</style>
