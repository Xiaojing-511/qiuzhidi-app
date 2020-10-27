<template>
	<view class="content">
		<view class="top">
			<view class="personal-information">
				<view class="name">
					<u-input placeholder-style="font-size:50rpx;color:black" v-model="value" type="text" placeholder="名字" disabled="true" />
					<u-input @click="goInformation" v-model="value" type="text" placeholder="查看并修改个人信息 >" disabled="true" />
				</view>
				<view class="avatar">
					<u-avatar :src="src" size="large" @click="goAavatar"></u-avatar>
				</view>
			</view>
			<view class="goTo">
				<u-icon label-size="24rpx" margin-top="20" class="do-marginA" name="file-text" size="70rpx" color="#00aaff" label="我的简历"
				 label-pos="bottom" @click="goResume"></u-icon>
				<u-line direction="col" color="#d3d3d3" length="90%" />
				<u-icon label-size="24rpx" margin-top="20" class="do-marginB" name="calendar" size="70rpx" color="#00aaff" label="我的投递"
				 label-pos="bottom" @click="goDeliver"></u-icon>
			</view>
		</view>
		<view class="bottom">
			<ul class="bottom-ul">
				<li @click="goOpinion">
					<u-icon name="edit-pen" margin-left="18" size="40rpx" color="#00aaff" label="意见反馈与帮助" label-size="26rpx"></u-icon>
				</li>
				<u-line color="#e6e3e3" length="95%" />
				<li @click="goNews">
					<u-icon name="email-fill" margin-left="18" size="40rpx" color="#00aaff" label="我的发布" label-size="26rpx"></u-icon>
				</li>
				<u-line color="#e6e3e3" length="95%" />
				<li @click="goAbout">
					<u-icon name="error-circle-fill" margin-left="18" size="40rpx" color="#00aaff" label="关于我们" label-size="26rpx"></u-icon>
				</li>
				<u-line color="#e6e3e3" length="95%" />
				<li @click="changeStatus">
					<u-icon name="account-fill" margin-left="18" size="40rpx" color="#00aaff" label="切换身份" label-size="26rpx"></u-icon>
				</li>
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
			}
		},
		methods: {
			goInformation() {
				this.$u.route('');
			},
			goAavatar() {
				this.$u.route('');
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
			goNews() {
				this.$u.route('');
			},
			goAbout() {
				this.$u.route('');
			},
			changeStatus() {
				this.$u.route('');
			}
		}
	}
</script>

<style lang="scss" scoped>
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

				.name {
					u-input {
						font-size: 30rpx;
					}
				}
			}

			.goTo {
				display: flex;
				padding-bottom: 40rpx;

				.do-marginA {
					margin-left: 150rpx;
					margin-right: 70rpx;
				}

				.do-marginB {
					margin-right: 150rpx;
				}
			}
		}

		.bottom {
			border-top: 20upx solid #f3f2f2;

			.bottom-ul {
				display: flex;
				flex-direction: column;

				u-line {
					margin-left: 30rpx;
				}

				li {
					line-height: 100rpx;

					u-icon {
						margin-left: 30rpx;
					}
				}

				li:not(:last-child)::after {
					content: '＞';
					font-size: 40rpx;
					color: $color_subhead_outline;
					float: right;
					margin-right: 20rpx;
				}
			}
		}

		.empty {
			height: 25vh;
			background-color: #f3f2f2;
		}
	}
</style>
