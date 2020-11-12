<template>
	<view class="content">
		<view class="iner_box">

			<view class="head_img">
				<u-upload ref="uUpload" :max-count='1' width='120rpx' height="120rpx" :file-list="fileList" :before-upload="beforeUpload"></u-upload>
			</view>
			<view class="first_box">
				<view class="tip_text">
					<text>基本信息</text>
				</view>
				<view class="msg">
					<view class="tip">
						昵称:<input class="" type="text" v-model="nickname" value="" />
					</view>
					<view class="tip">
						职位:<input type="text" v-model="job" value="" />
					</view>
					<view class="tip">
						工龄:<input type="text" v-model="gender" value="" />
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

					<textarea class="text-area" v-model='workExperience' cols="" rows="" style="vertical-align:top;outline:none;"></textarea>
				</view>
			</view>
			<view class="third_box">
				<view class="tip_text">
					<text>面试侧重点</text>

					<textarea class="text-area" v-model="focus" cols="" rows="" style="vertical-align:top;outline:none;"></textarea>
				</view>

				<view class="btn">
					<u-button :custom-style="customStyle1" size="default" @click="reload" type="default">以后再填</u-button>

					<u-button :custom-style="customStyle2" size="default" @click="postInfo" type="primary">发布</u-button>

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


				customStyle1: {
					marginLeft: '25rpx',
					paddingLeft: '90rpx',
					paddingRight: '90rpx',
				},

				customStyle2: {
					marginLeft: '20rpx',
					paddingLeft: '120rpx',
					paddingRight: '120rpx',
				},


			}
		},
		onLoad() {
			this.getInfo()
			console.log(this)
		},

		methods: {

			beforeUpload(index, list) {
				// 只上传偶数索引的文件
				console.log(list[0].url)
				let that = this
				uni.uploadFile({
					url: 'http://118.24.96.51:8085/file/single', // 后端api接口
					filePath: list[0].url, // uni.chooseImage函数调用后获取的本地文件路劲
					sizeType: 'compressed',
					formData: {
						dir: 'static/img'
					},
					name: 'file', //后端通过'file'获取上传的文件对象
					header: {
						Authorization: `Bearer` + that.vuex_token
					},
					success: (res) => {
						that.icon = JSON.parse(res.data).data
						console.log(that.icon)

					}
				})
			},
			getInfo() {
				this.$u.post('/interviewer/details', {

				}).then((res) => {
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
					let a = []
					a.push({
						url: 'http://118.24.96.51:8085/' + res.icon
					})
					this.fileList = a

				})



			},


			postInfo() {
				this.$u.post('/interviewer/submit/changes', {
					nickname: this.nickname,
					icon: this.icon,
					gender: this.gender,
					city: this.city,
					job: this.job,
					personalizedSignature: this.personalizedSignature,
					workExperience: this.workExperience,
					focus: this.focus,

				}).then((res) => {

					this.showToasts('发布成功')
				}).catch((err) => {
					this.showToasts('请求失败')
				})
			},

			showToasts(msg, type = 'warning') {
				this.$refs.uToast.show({
					title: msg,
					type: type,
					position: 'top'
				})
			},

			reload() {
				console.log('ss')
				uni.redirectTo({
					url: '/pages/interviewer_editor/interviewer_editor',
					// uni.navigator({

					// })
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

	.btn {
		display: flex;
	}

	button {
		// margin-left: 0rpx;
		// margin-right: 0rpx;
		// padding-left: 150rpx;
		// padding-right: 150rpx;
		// line-height: ;
	}

	.btn1 .btn2 {

		width: 300rpx;
		height: 100%;
	}
</style>
