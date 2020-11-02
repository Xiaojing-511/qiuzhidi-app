<template>
	<view class="container">
		<view class="list-select" style="position:fixed;top:0;">
			<view class="list-item" v-for="(item,index) in list" :key="index" :class="{active:current==index}" :data-current="index"
				  @tap="tabChange">
				<view class="list-name">
					{{item.name}}
					{{index!=0? `(${list[index].totalNum})`: ''}}
				</view>
				<view :class="{activeLine:current==index}"></view>
			</view>
		</view>

		<view class="swiper" style="margin: 120rpx 0 0;">
			<view class="swiper-item" :class="{isShow:current==index}" v-for="(item,index) in infoList" :key="index">
				<view v-for="(info,index) in item" :key="index" :data-userinfo="info" @click="navToDetail">
					<view class="list-item">
						<view class="list-item-between">
							<view style="font-size:34rpx;color: #333333;">
								<!-- Web前端开发工程师 -->
								{{info.recruit_name}}
							</view>
							<view style="font-size: 30rpx;color: #FF0000;">
								<!-- 20k-30k -->
								{{info.recruit_salary}}
							</view>
						</view>

						<view class="list-item-between">
							<view class="item-tag" style="display: flex;">
								<view class="item-info-tag">
									<!-- 北京 -->
									{{info.recruit_workplace}}
								</view>
								<view class="item-info-tag">
									<!-- 校招 -->
									{{typeList[info.type]}}
								</view>
							</view>
							<view style="font-size: 20rpx;color: #999999;">
								<!-- 2018-05-11 -->
								{{info.release_date}}
							</view>
						</view>

						<view class="list-item-detail">
							<view class="detail-img">
								<img class="detail-img-item" style="width: 100rpx; height: 100rpx;" :src="imgUrl+info.company_log"></img>
							</view>
							<view class="detail-info">
								<view style="font-size: 30rpx;">
									<!-- 阿里巴巴 -->
									{{info.company_name}}
								</view>
								<view class="list-item-between">
									<view style="font-size: 24rpx;">
										<!-- D轮及以上/1000-9999人 -->
										{{info.company_intro}}
									</view>
									<view style="font-size: 24rpx;color: #00A8FF;">
										<!-- 录用 -->
										{{current==0? '' : list[info.status].name}}
									</view>
								</view>
							</view>
						</view>

					</view>
				</view>
			</view>
			<u-toast ref="uToast" />
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				statusCount: 5, //状态数量
				current: 0, //当前index
				showTotalNum: true, //是否将总数渲染到页面
				imgUrl: 'http://47.99.121.209:8105/', //图片头地址
				list: [{
					name: "全部",
					pageNum: 0,
					totalNum: 0,
					canGetinfo: true, //滑动到底部是否获取数据 （false:全部数据已获取）
				},
					{
						name: "待选",
						pageNum: 0,
						totalNum: 0,
						canGetinfo: true
					},
					{
						name: "面试中",
						pageNum: 0,
						totalNum: 0,
						canGetinfo: true
					},
					{
						name: "通过",
						pageNum: 0,
						totalNum: 0,
						canGetinfo: true
					},
					{
						name: "失败",
						pageNum: 0,
						totalNum: 0,
						canGetinfo: true
					}
				], //选项卡列表
				typeList: ["实习", "校招", "社招"],
				infoList: {
					0: [],
					1: [],
					2: [],
					3: [],
					4: []
				}
			};
		},
		methods: {
			getInfo() { //获取信息
				this.list[this.current].canGetinfo = false;
				if (this._data.current == 0) {
					this.$u.post('dfb/findAll', {
						// status: this._data.current,
						currentPageNum: ++this.list[this._data.current].pageNum,
						pageSize: 5
					}).then(res => {
						// console.log("res",res);
						if (!res) {
							// this.showToast("全部加载完毕了哦~")
							this.list[this.current].canGetinfo = false;
						} else {
							// console.log(this.list[this._data.current].pageNum);
							this.infoList[this._data.current] = this.infoList[this._data.current].concat(res.list);
							this.list[this.current].canGetinfo = true;
						}

						// 将token存储在vuex中（用的uview优化过的写法，vuex_token字段配置了，可以直接存在了本地存储中）
						// this.$u.vuex('vuex_token', res.token)
						// 返回原本所在页面
						// uni.navigateBack()
					}).catch(err => {
						console.log('error:', err)
					})
				} else {
					this.$u.post('dfb/findDeliverByStatus', {
						status: this._data.current,
						currentPageNum: ++this.list[this._data.current].pageNum,
						pageSize: 5
					}).then(res => {
						// console.log(res.list);
						if (!res) {
							// this.showToast("全部加载完毕了哦~")
							this.list[this.current].canGetinfo = false;
						} else {
							// console.log(this.list[this._data.current].pageNum);
							this.infoList[this._data.current] = this.infoList[this._data.current].concat(res.list);
							this.list[this.current].canGetinfo = true;
						}

					}).catch(err => {
						console.log('error:', err)
					})
				}

			},
			navToDetail(e) {
				console.log(e.currentTarget.dataset.userinfo);
			},

			tabChange: function(e) {
				var index = e.target.dataset.current || e.currentTarget.dataset.current;
				this.current = index;
				if (this.list[this.current].canGetinfo) {
					this.getInfo();
				}
			},
			showToast(msg) {
				this.$refs.uToast.show({
					title: msg,
					position: 'bottom'
				})
			}
		},
		onLoad() {
			// console.log(this.vuex_token);
			this.$u.post('dfb/totalOfStatus')
					.then(res => {
						// console.log(res);
						for (let item in res) {
							this.list[item].totalNum = res[item];
						}

					}).catch(err => console.log(err))

			this.getInfo();
		},
		onReachBottom() {
			if (this.list[this.current].canGetinfo) {
				this.showToast("loading...");
				this.getInfo();
			}
		}
	};
</script>

<style lang="scss">
	.container {
		width: 100%;
		background-color: #EEEEEE;

		.list-select {
			z-index: 99;
			width: 100%;
			height: 100rpx;
			background-color: #FFFFFF;
			display: flex;
			border-top: 2rpx solid #EEEEEE;
			justify-content: space-around;

			.list-item {
				width: 20%;
				font-size: 28rpx;
				text-align: center;
				color: #999999;

				.list-name {
					margin-top: 50rpx;
					transform: translateY(-50%);
				}

				.activeLine {
					width: 100rpx;
					height: 2rpx;
					margin: 8rpx auto 0;
					background-color: #00A8FF;
				}
			}

			.active {
				color: #444D54;
				font-size: 30rpx;
			}
		}

		.swiper-item {
			display: none;

			.list-item {
				height: 276rpx;
				background-color: #FFFFFF;
				margin: 20rpx 0;
				padding: 30rpx;

				.list-item-detail {
					display: flex;
					align-items: flex-end; //沿底部对齐
				}

				.list-item-between {
					display: flex;
					justify-content: space-between;
					margin: 8rpx 0;

					.item-info-tag {
						color: #999999;
						background-color: #EEEEEE;
						font-size: 20rpx;
						border-radius: 6rpx;
						margin-right: 10rpx;
						padding: 5rpx;
					}
				}

				.list-item-detail {

					.detail-info {
						width: 100%;
						margin-left: 30rpx;
					}
				}
			}
		}

		.isShow {
			display: block;
		}
	}
</style>
