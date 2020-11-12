<template>
	<view class="content">
		<view class="iner_box">

			<view class="head_img">
				<u-image width="120rpx" height="120rpx" :src="'http://118.24.96.51:8085/' + icon" mode=""></u-image>
			</view>
			<view class="first_box">
				<view class="tip_text">
					<text>基本信息</text>
				</view>
				<view class="msg">
					<view class="tip">
						昵称:<text>{{nickname}}</text>
					</view>
					<view class="tip">
						职位:<text>{{job}}</text>
					</view>
					<view class="tip">
						工龄:<text>{{gender}}</text>
					</view>

					<view class="tip">
						公司:<text>{{companyName}}</text>
					</view>
					<view class="tip">
						评分：<u-rate active-color="#FA3534" :current="4" :disabled="true" inactive-color="#b2b2b2" gutter="20"></u-rate>
					</view>
				</view>
			</view>
			<view class="second_box">
				<view class="tip_text">
					<text>工作经历</text>

				</view>
				<view class="msg">
					<text>{{workExperience}}</text>
				</view>
			</view>
			<view class="third_box">
				<view class="tip_text">
					<text>面试侧重点</text>
				</view>
				<view class="msg">
					<text>{{focus}}</text>
				</view>

			</view>
		</view>

	</view>
</template>
<script>
	export default {
		data() {
			return {
				nickname: "",
				icon: '',
				companyName: '',
				gender: 0,
				city: '',
				job: '',
				personalizedSignature: '',
				workExperience: '',
				focus: '',
				fileList: [],
			}
		},
		onLoad() {
			this.getInfo()
		},

		methods: {
			getInfo() {
				this.$u.post('/interviewer/details', {}).then((res) => {
					console.log(res);
					this.nickname = res.data.nickname;
					this.icon = res.data.icon;
					this.gender = res.data.gender;
					this.city = res.data.city;
					this.job = res.data.job;
					this.personalizedSignature = res.data.personalizedSignature;
					this.workExperience = res.data.workExperience;
					this.focus = res.data.focus;
					this.companyName = res.data.companyName;
					this.icon = res.data.icon;
				})
			},
		}
	}
</script>

<style lang="scss" scoped>
	.contect {
		height: 100%;
		width: 100%;
		position: absolute;
	}

	.iner_box {
		display: block;
		width: 600rpx;
		margin: 80rpx 40rpx;

	}

	.tip_text {
		font-size: 36rpx;

		font-family: PingFangSC;
		font-weight: 600;
		color: #333333;

	}

	.msg {
		margin-top: 60rpx;
		margin-left: 60rpx;

	}

	.msg text {
		display: flex;
	}

	.tip {
		display: flex;
		margin-bottom: 20rpx;

		font-size: 30rpx;
		font-family: PingFang;
		font-weight: 500;
		color: #333333;
	}

	.text-area {
		margin-top: 20rpx;

		font-size: 30rpx;
		font-family: PingFang;
		font-weight: 500;
		color: #999999;
	}

	.head_img {
		position: absolute;
		right: 80rpx;
		top: 180rpx;
	}

	.upld {
		display: flex;
	}

	.msg>text {
		margin-top: -45rpx;
		margin-bottom: 15rpx;

		font-size: 30rpx;
		font-family: PingFang;
		font-weight: 500;
		color: #333333;
	}
</style>
