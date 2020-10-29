<template>
	<view class="content">
		<view class="white-back">
			<view class="back-img">
				<u-search shape="round" :clearabled="false" :animation="false" :show-action="false" height="64" placeholder="JAVA工作"
							 v-model="searchWord" bg-color='#F2F2F2' @click="goSearch" :disabled="true" ></u-search>
							<view class="swiper-imgs">
								<u-swiper :list="imgs" class="inner-imgs" height='300'></u-swiper>
							</view>
			</view>
			
					
			<view class="dtlbox">
				<view class="" @click="goInner">
					<image src="../../static/img/WechatIMG752.png" class="dtl-img"></image>
				</view>
				<view @click="goSchool">
					<image src="../../static/img/WechatIMG751.png" class="dtl-img"></image>
				</view>
				<view @click="goSocial">
					<image src="../../static/img/WechatIMG745.png" class="dtl-img last-img"></image>
				</view>
			</view>
			<view class="tab-box ">
				<u-tabs name="cate_name" count="cate_count" :list="list" :is-scroll="false" :current="current" @change="change"
				 :show-bar='false' font-size="30" class="tabs" active-color='#333333' inactive-color='#999999'></u-tabs>
			</view>
		</view>


		<view class="comp_list " v-show="current===0">
			<view v-for="(item,index) in interpolateList" :key="item.companyId" @click="goDetail" class="detail-circulate">
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
			<view v-for="(item,index) in schoolList" :key="item.companyId" @click="goDetail" class="detail-circulate">
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
			<view v-for="(item,index) in socialList" :key="item.companyId" @click="goDetail" class="detail-circulate">
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
			this.fitstGet();

		},
		onReachBottom() {
			this.upList();
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
				innerIsFirst: true, //是否是第一次获取
				schoolPageNum: 1,
				schoolIsLast: false,
				schoolIsFinsh: true,
				schoolIsFirst: true, //是否是第一次获取
				socialPageNum: 1,
				socialIsLast: false,
				socialIsFinsh: true,
				socialIsFirst: true, //是否是第一次获取
				current: 0,
				imgs: [],
				searchWord: '',
				showList: [],
				interpolateList: [],
				schoolList: [],
				socialList: [],
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
				this.upList()
			},
			goSearch() {
				uni.navigateTo({
					url: ''
				})
			},
			fitstGet() {
				if (this.current == 0 && this.innerIsFirst) {
					this.getInterpolateList()
					this.innerIsFirst = false
				} else if (this.current == 1 && this.schoolIsFirst) {
					this.getSchoolList()
					this.schoolIsFirst = false
				} else if (this.current == 2 && this.socialIsFirst) {
					this.getSocialList()
					this.socialIsFirst = false
				}
			},
			upList() {
				if (this.current == 0 && this.innerIsFinsh && !this.innerIsLast) {
					this.getInterpolateList()
					console.log(this.innerPageNum)

				} else if (this.current == 1 && this.schoolIsFinsh && !this.schoolIsLast) {
					this.getSchoolList()

				} else if (this.current == 2 && this.socialIsFinsh && !this.socialIsLast) {
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
						this.innerPageNum++;

						console.log(this.interpolateList)
					}).catch((res) => {
						this.showList(res.message)
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
						this.socialPageNum++;
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
			goInner() {
				uni.navigateTo({
					url: ''
				})
			},
			goSchool() {
				uni.navigateTo({
					url: ''
				})
			},
			goSocial() {
				uni.navigateTo({
					url: ''
				})
			},
			goDetail() {
				uni.navigateTo({
					url: ''
				})

			},
			goSearch(){
				console.log('111')
				uni.navigateTo({
					url: '../searchDetail/searchDetail'
				})
			}


		}
	}
</script>

<style lang="scss" scoped>
	.white-back {
		background-color: #FFFFFF;
		margin-left: -30rpx;
		margin-right: -30rpx;
		padding: 0 30rpx;
	}

	.content {
		width: 100%;
		padding: 0 30rpx;
		background-color: #DDDDDD;
	}

	.swiper-imgs {
		margin-top: 40rpx;
		height: 300rpx;
	}

	.dtlbox {
		display: flex;
		-webkit-justify-content: space-around;
		justify-content: space-around;
		margin: 30rpx auto;
		margin-bottom: 0rpx;
	}

	.dtl-img {
		height: 125rpx;
		width: 220rpx;
		margin-right: 15rpx;

	}

	.last-img {
		margin-right: 0rpx;
	}


	.back-img {
	background-image: url(../../static/img/WechatIMG766.png);
	margin-left: -30rpx;
	margin-right: -30rpx;
	padding: 0 30rpx;
	background-size:750rpx 360rpx;
	background-repeat: no-repeat;
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

	.comp_list {
		// background-color: #DDDDDD;
		margin-left: -30rpx;
		margin-right: -30rpx;
		padding: 0 30rpx;
	}

	.detail-title {
		display: flex;
		justify-content: space-between;
		padding-top: 20rpx;
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
		border-radius: 10rpx;
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
