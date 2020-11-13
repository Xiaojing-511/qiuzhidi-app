<template>
	<view>
		<view class='header'>
			<view class="recruitName">{{recruitName}}</view>
			<view class="recruitSalary">{{recruitSalary}}</view>
			<view class='part'>
				<view class="recruitWorkspace">{{recruitWorkspace}}</view>
				<view class="type">{{recruitType}}</view>
			</view>
			<view class='releaseDate'>{{releaseDate}}</view>
		</view>

		<view class='nav'>
			<cover-image class="image"  :src='companyLogo'></cover-image>
			<view class="companyName">{{companyName}}</view>
			<view class="companyAddress">{{companyAddress}}</view>
			<view class='line'></view>
		</view>

		<view class='article'>
			<view class='text1'>工作要求:</view>
			<view class="skillRequired">{{skillRequired}}</view>
			<view class='text2'>企业介绍:</view>
			<view class="companyIntro">{{companyIntro}}</view>
			<view class='text3'>补充说明:</view>
			<view class="jobDescription">{{jobDescription}}</view>
		</view>
		<button class='deliver' @click="deliver()">投递</button>
		<view class='bottom'></view>
	</view>
</template>

<script>
	export default {
		data() {
			return {

				recruitType: '', //自定义招聘类型
				companyAddress: '', //地址
				companyLogo: '', //图片
				companyName: '', //公司
				recruitName: '', //职位
				recruitSalary: '', //薪资
				recruitWorkspace: '', //部门
				type: '', //招聘类型
				skillRequired: '', //工作要求
				companyIntro: '', //企业介绍
				jobDescription: '', //补充描述
				releaseDate: '', //发布时间
				companyId: '',
				id: '',
				imgUrl: 'http://www.qingmengtech.com:8085/', //图片路径
			}
		},
		created() {

		},
		methods: {
			onLoad() {

				this.$u.post('recruit/getRecruitDetail', {
					recruitId: 1
				}).then(res => {
					console.log(res);
					this.companyName = res.companyName,
						this.companyAddress = res.companyAddress,
						this.companyIntro = res.companyIntro,
						// this.companyLogo = 'http://www.qingmengtech.com:8085/'+'uploads/di/20201027180138_20327.jpg',
						this.companyLogo = this.imgUrl + res.companyLogo,
						this.id = res.id,
						this.type = res.type
					    this.recruitName = res.recruitName,
						this.recruitSalary = res.recruitSalary,
						this.recruitWorkspace = res.recruitWorkspace,
						this.jobDescription = res.jobDescription,
						this.companyId = res.companyId,
						this.skillRequired = res.skillRequired
					this.releaseDate = res.releaseDate
					this.releaseDate = this.releaseDate.substring(0, 11);
					if (res.type == 0) {
						this.recruitType = '实习'
					} else if (res.type == 1) {
						this.recruitType = '校招'
					} else {
						this.recruitType = '社招'
					}
					// console.log(this.$scope.globalData.url)
				}).catch(res => {
					console.log('error')
				})
			},
			deliver() {
				this.$u.route({
					url: 'pages/recuritmentDetails/recuritment'
				})
			},
		},


	}
</script>

<style lang="scss" scoped>
	page {
		margin: 0;
		padding: 0;
		// position: relative;
	}
	.header {
		margin-left: $font_important_small;
		margin-top: $font_important_small;
		// border: 1px solid black;
		width: 690rpx;
		.recruitName {
			font-weight: 600;
			font-size: 36rpx;
		}
		.recruitSalary {
			color: #ff0000;
			font-size: 30rpx;
			position: absolute;
			left: 600rpx;
			top: $font_important_small;
		}
		.releaseDate {
			margin-left: 575rpx;
			margin-top: -30rpx;
			color: #999999;
			font-size: 20rpx;
		}
		.part {
			// border:1rpx black solid;
			width:240rpx;
			line-height: -3rpx;
			text-align: center;
			font-size: 20rpx;
			color: $color_subhead_outline;
			.recruitWorkspace {
				background-color: $color_content;
				// margin-left: 31rpx;
				margin-top: 19rpx;
				height: 32rpx;
				width: 60rpx;
			}
			.type {
				background-color: $color_content;
				height: 32rpx;
				width: 60rpx;
				margin-left: 70rpx;
				margin-top: -32rpx;
			}
		}
	}
	.nav{
		// border: 1px black solid;
		margin-left: 30rpx;
		margin-top: 20rpx;

		.image {
			width: 100rpx;
			height: 100rpx;
			border-radius: 10rpx;
		}

		.companyName {
			font-size: 30rpx;
			margin-left: 129rpx;
			margin-top: -95rpx;
		}

		.companyAddress {
			font-size: 24rpx;
			margin-left: 128rpx;
			margin-top: 19rpx;
		}

		.line {
			width: 691rpx;
			height: 1rpx;
			background-color: $color_outline;
			margin-top: 20rpx;
			// border:0.5rpx #dddddd solid;
		}
	}
	.article {
		// border:1rpx black solid;
		color: $color_title;
		font-size: 36rpx;
		margin-left: 33rpx;
       
		.text1 {
			margin-top: 50rpx;
			font-weight: 600;
		}

		.text2 {
			margin-top: 83rpx;
			font-weight: 600;
		}

		.text3 {
			margin-top: 84rpx;
			font-weight: 600;
		}

		.skillRequired,
		.companyIntro,
		.jobDescription {
			font-size: 30rpx;
			margin-left: 80rpx;
			margin-top: 50rpx;
			width: 600rpx;
			// border:1px black solid;
		}
	}
	.deliver {
		font-size: $font_important_small;
		color: #ffffff;
		line-height: 100rpx;
		width: 599rpx;
		height: 90rpx;
		margin-top: 163rpx;
		margin-left: 64rpx;
		background-color: $color_emphasize;
		border-radius: 5rpx;
	}
	.bottom {
		width: 229.6rpx;
		height: 7.6rpx;
		margin-left: 260.2rpx;
		margin-top: 37.4rpx;
		background-color: rgba(0, 0, 0, 0.5);
		margin-bottom: 23rpx;
	}
</style>
