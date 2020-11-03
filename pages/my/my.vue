<template>
	<view class="content">
		<view class="top">
			<view class="personal-information">
				<view class="name">
					<u-input v-model="name"  :custom-style="{fontSize:' 50rpx'}"  placeholder-style="font-size: 50rpx" disabled="true"/>
					<u-input @click="goInformation" placeholder="查看并修改个人信息>" disabled="true" />
				</view>
				<view class="avatar">
					<u-avatar :src="src" size="large" @click="goAavatar"></u-avatar>
				</view>
			</view>
			<view>
				<u-modal v-model="show" title=" " show-cancel-button @confirm="confirm" >
					<!-- <u-input v-model="content" input-align="center" /> -->
					<u-field v-model="content" placeholder="请输入你要修改的昵称"	></u-field>
				</u-modal>
			</view>
			<view class="goTome">
				<view @click="goResume">
					<i class="iconfont bigIcon icon-jibenxinxi"></i>
					<span>基本信息</span>
				</view>
				<view class="line">
					<u-line direction="col" color="#d3d3d3" length="90%" />
				</view>
				<view @click="goDeliver">
					<i class="iconfont bigIcon icon-toudijianli"></i>
					<span>我的投递</span>
				</view>
			</view>
		</view>
		<view class="bottom">
			<ul class="bottom-ul">
				<li @click="goOpinion">
					<i class="iconfont icon-image_yijianyufankui"></i>
					<span>意见反馈与帮助</span>
				</li>
				<u-line color="#e6e3e3" length="95%" />
				<li @click="goDistribution">
					<i class="iconfont icon-svg"></i>
					<span>我的发布</span>
				</li>
				<u-line color="#e6e3e3" length="95%" />
				<li @click="goAbout">
					<i class="iconfont icon-gantanhao"></i>
					<span>关于我们</span>
				</li>
				<u-line color="#e6e3e3" length="95%" />
				<li @click="changeStatus">
					<i class="iconfont icon-qiehuanshenfen"></i>
					<span>切换身份</span>
				</li>
				<u-line color="#e6e3e3" length="95%" />
			</ul>
		</view>
		<view class="empty">

		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				src: 'http://pic2.sc.chinaz.com/Files/pic/pic9/202002/hpic2119_s.jpg',
				id: 83,
				name: '李嘉申',
				show: false,
				content: ''
			}
		},
		methods: {
			goInformation() {
				this.show = true;
			},
			confirm() {
				this.$u.vuex('vuex_token',
					"eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJzdHU5NTc3IiwiY3JlYXRlZCI6MTYwNDI5NTcxNTY4MSwiZXhwIjoxNjA0OTAwNTE1fQ.SXm4oSIkJpfeP1t0gPJWthbU1_EjoI9_Km_DLfQOlsNzopDa_WIAwifT0w2tbKb9zETUvCGJgcYeEb5HnVOkbg"
				)
				this.$u.post(`http://47.99.121.209:8093/member/info/updateNickname/${this.id}`, {
					nickname: this.content
				}).then(res => {
					this.name = this.content;
					uni.showToast({
						title: "修改成功",
						duration: 2000,
					});
				});
			},
			goAavatar() {
				// this.$u.vuex('vuex_token',
				// 	"eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJzdHU5NTc3IiwiY3JlYXRlZCI6MTYwNDI5NTcxNTY4MSwiZXhwIjoxNjA0OTAwNTE1fQ.SXm4oSIkJpfeP1t0gPJWthbU1_EjoI9_Km_DLfQOlsNzopDa_WIAwifT0w2tbKb9zETUvCGJgcYeEb5HnVOkbg"
				// )
				// this.$u.post(`http://47.99.121.209:8093/member/info/updateIcon/${this.id}`, {
				// 	icon: this.id
				// }).then(res => {
				// 	this.name = this.content;
				// 	uni.showToast({
				// 		title: "修改成功",
				// 		duration: 2000,
				// 	});
				// });
			},
			goResume() {
				this.$u.route('');
			},
			goDeliver() {
				this.$u.route('');
			},
			goOpinion() {
				this.$u.route('pages/feedback/feedback');
			},
			goDistribution() {
				this.$u.route('pages/myDistribution/myDistribution');
			},
			goAbout() {
				this.$u.route('pages/');
			},
			changeStatus() {
				this.$u.route('');
			}
		}
	}
</script>

<style lang="scss" scoped>
	.iconfont {
		font-size: 32rpx;
		color: #073e5a;
	}

	.content {
		background-color: #fff;
		border-top: 1rpx solid #d3d3d3;
		padding-top: 100rpx;
		width: 100vw;
		height: 100vh;

		.top {
			.personal-information {
				display: flex;
				justify-content: space-between;
				margin-bottom: 100rpx;
				padding: 0 30rpx;
			}

			.goTome {
				display: flex;
				justify-content: space-around;
				padding-bottom: 40rpx;

				.bigIcon {
					font-size: 48rpx;
					text-align: center;
					margin-bottom: 18rpx;
				}

				span {
					font-size: 24rpx;
				}
			}
		}

		.bottom {
			border-top: 20rpx solid #f3f2f2;

			.bottom-ul {
				display: flex;
				flex-direction: column;

				u-line {
					margin-left: 30rpx;
				}

				li {
					height: 100rpx;
					i {
						line-height: 100rpx;
						display: inline;
						margin: 0rpx 20rpx;
					}

					span {
						font-size: 26rpx;
						
					}
				}

				li:not(:last-child)::after {
					content: '＞';
					line-height: 100rpx;
					font-size: 40rpx;
					color: $color_subhead_outline;
					float: right;
					margin-right: 20rpx;
				}
			}
		}

		.empty {
			height: 30vh;
			background-color: #f3f2f2;
		}
	}
</style>
