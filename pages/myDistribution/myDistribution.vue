<template>
	<view class="content">
		<view class="tab-box">
			<u-tabs :list="list" :is-scroll="false" :current="current" @change="change"></u-tabs>
		</view>
		<view class="content">
			<view v-show="current === 0">
				<ul v-for="(item,index) in interview.list" :key="item.id" class='detail-block'>
					<li class='detail-title'>
						<text>{{item.recruitName}}</text>
						<span>{{item.recruitSalary}}</span>
					</li>
					<li class='detail-timer'>
						<span>{{item.recruitWorkspace}}</span>
						<span>{{item.releaseDate}}</span>
					</li>
					<li class='detail-imgs-box'>
						<image :src="'http://118.24.96.51:8085/'+item.companyLogo" mode="" class="detail-imgs"></image>
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
			<view v-show="current === 1">
				<ul v-for="(item,index) in recruit.list" :key="item.id" class='detail-block'>
					<li class='detail-title'>
						<text>{{item.recruitName}}</text>
						<span>{{item.recruitSalary}}</span>
					</li>
					<li class='detail-timer'>
						<span>{{item.recruitWorkspace}}</span>
						<span>{{item.releaseDate}}</span>
					</li>
					<li class='detail-imgs-box'>
						<image :src="'http://118.24.96.51:8085/'+item.companyLogo" mode="" class="detail-imgs"></image>
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
		onLoad(){
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
			firstGet(){
				if(this.current == 0 && this.interview.isFirst){
					this.getInterview()
					this.interview.isFirst=false
				}else if(this.current == 1 && this.recruit.isFirst){
					this.getRecruit()
					this.recruit.isFirst=false
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
					this.$u.post('/recruit/information', {
						type: this.current,
						pageNum: this.interview.pageNum
					}).then((res) => {
						this.interview.isFinsh = true;
						this.interview.pageNum++;
						this.interview.list = this.interview.list.concat(res.list);
						this.interview.isLast = res.isLastPage
						console.log(this.interview.list)
					})
				}

			},
			getRecruit() {
				if (this.recruit.isFinsh) {
					this.recruit.isFinsh=false;
					this.$u.post('/recruit/information', {
						type: this.current,
						pageNum: this.recruit.pageNum
					}).then((res) => {
						this.recruit.isFinsh = true;
						this.recruit.pageNum++;
						this.recruit.list = this.recruit.list.concat(res.list);
						this.recruit.isLast = res.isLastPage
						console.log(this.recruit.list)
					})
				}
			},
			postAgain(){
				if(this.current==0){
					
				}
				if(this.current==1){
					
				}
			}
		}
	}
</script>

<style>
	.content{
		width: 100%;
	}
.btn{
	position: fixed;
	bottom: 100rpx;
	left: 260rpx;
	z-index: 9;
	background-color:#007AFF;
	
}
.detail-block{
	margin-bottom: 20rpx;
	background-color: #C8C9CC;
	height: 300rpx;
	
}
.detail-imgs{
width: 100rpx;
height: 100rpx;
}
.detail-imgs-box{
	display: flex;
}
.tab-box{
	width: 50%;
}
</style>
