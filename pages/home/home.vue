<template>
	<view class="content">
		<u-swiper :list="imgs"></u-swiper>
		<view class="dtlbox">
			<view class="">
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
			<input type="text" class="input-search" placeholder="请输入" v-model="seachText" :placeholder-style="inp_src" />
			<u-button :custom-style="customStyle" shape="circle">搜索</u-button>

		</view>


		<!-- <view class="u-border-bottom"> -->
		<view class="search-bar" v-for="(item,index) in showList" :key='index'>

		</view>
		<!-- </view> -->



		<view class="tab-box ">
			<u-tabs name="cate_name" count="cate_count" :list="list" :is-scroll="false" :current="current" @change="change"
			 font-size="30" class="tabs"></u-tabs>
		</view>

		<view class="line">

		</view>



		<view class="comp_list " v-show="current===0">
			<view v-for="(item,index) in interpolateList" :key="item.companyId" >
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
		<view class="comp_list " v-show="current===1">
			<view v-for="(item,index) in schoolList" :key="item.companyId" >
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
		
		<view class="comp_list " v-show="current===2">
			<view v-for="(item,index) in socialList" :key="item.companyId">
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
	</view>
</template>

<script>
	export default {
		onLoad() {
			this.getimgs();
			this.getInterpolateList();
			
		},
		onReachBottom() {
			this.upDatalist()
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
				innerPageNum: 1,
				innerIsLast: false,
				innerIsFinsh: true,
				schoolPageNum: 1,
				schoolIsLast: false,
				schoolIsFinsh: true,
				socialPageNum: 1,
				socialIsLast: false,
				socialIsFinsh: true,
				current: 0,
				imgs: [],
				seachText: '',
				searchList: [],
				showList: [],
				interpolateList: [],
				schoolList:[],
				socialList:[],
				customStyle: {
					width: '120rpx',
					color: '#FFFFFF',
					fontSize: '30rpx',
					background: '#00A8FF'

				},
				// input_style: 'color:#FFFFFF;font-size:30rpx;',
				inp_src: 'text-align:center',

			}
		},
		methods: {
			change(index) {
				this.current = index;
				console.log(this.current);
				this.changeList()
			},
			changeList(){
				if(this.current==0&&this.innerIsFinsh&&!this.innerIsLast){
					this.getInterpolateList()
					
				}else if(this.current==1&&this.schoolIsFinsh&&!this.schoolIsLast){
					this.getSchoolList()
					
				}else if(this.current==2&&this.socialIsFinsh&&!this.socialIsLast){
					this.getSocialList()
					
				}
			},
			getInterpolateList() {
				if (this.innerIsFinsh) {
					this.innerIsFinsh = false
					this.$u.post('/recruit/information', {
						type: this.current,
						pageNum: this.innerPageNum
					}).then((res) => {
						console.log(res)
						this.interpolateList = this.interpolateList.concat(res.list);
						this.innerIsFinsh = true;
						this.innerIsLast = res.isLastPage;
						this.innerPageNum++
						console.log(this.interpolateList)
					})
				}
			},
			getSchoolList() {
				if (this.schoolIsFinsh) {
					this.schoolIsFinsh = false
					this.$u.post('/recruit/information', {
						type: this.current,
						pageNum: this.schoolPageNum
					}).then((res) => {
						console.log(res)
						this.schoolList = this.schoolList.concat(res.list);
						this.schoolIsFinsh = true;
						this.schoolIsLast = res.isLastPage;
						this.schoolPageNum++;
						console.log(this.schoolList)
					})
				}
			},
			getSocialList() {
				if (this.socialIsFinsh) {
					this.socialIsFinsh = false
					this.$u.post('/recruit/information', {
						type: this.current,
						pageNum: this.socialPageNum
					}).then((res) => {
						console.log(res)
						this.socialList = this.socialList.concat(res.list);
						this.socialIsFinsh = true;
						this.socialIsLast = res.isLastPage;
						this.socialPageNum++
						console.log(this.socialList)
					})
				}
			},
			getimgs() {
				this.$u.get('/recruit/rotation').then((res) => {
					this.imgs = res.map((value) => {
						return 'http://118.24.96.51:8085/' + value.url;
					})
					console.log(this.imgs)
				})
			},
			upDatalist() {
				this.changeList()
			}

		}
	}
</script>

<style lang="scss" scoped>
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
		margin: 40rpx auto;
	}

	.dtl-img {
		height: 100rpx;
		width: 100rpx;

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
		padding: 0 26rpx;
		margin-bottom: 26rpx;

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

	.comp_list {

		padding: 0 26rpx;
	}

	.detail-title span {
		margin: auto 10rpx;
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

	.u-border-bottom {
		display: flex;

	}

	.line {
		margin-top: 10rpx;
		border: 1rpx solid rgba($color: #666666, $alpha: 0.1);
	}

	ul {
		padding-inline-start: 0rpx;
	}
</style>
