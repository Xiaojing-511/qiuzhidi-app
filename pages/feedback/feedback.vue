<template>
	<view class="box">
		<view>
			<p class="p1" @click="goHistory">历史反馈</p>
		</view>
		<view class="content">
			<view class="text">反馈类型</view>
			<u-line color="#c8c9cc"></u-line>
			<u-radio-group v-model="value" @change="radioGroupChange">
				<u-radio @change="radioChange" v-for="(item, index) in list" :key="index" :name="item.type" :disabled="item.disabled">
					{{item.name}}
				</u-radio>
			</u-radio-group>
		</view>


		<view class="content">
			<view class="text">反馈意见</view>
			<u-line color="#c8c9cc"></u-line>
			<u-field v-model="txt" label=" " placeholder="您得反馈对我们很重要,请在此输入." type="textarea">
			</u-field>
			<u-upload  ref="uUpload" max-count="5" :file-list="fileList" :before-upload="beforeUpload"></u-upload>
		</view>


		<view class="content">
			<view class="text">联系电话</view>
			<u-line color="#c8c9cc"></u-line>
			<u-field v-model="mobile" label=" " placeholder="留下你的电话方便我们联系你哦~">
			</u-field>
		</view>


		<view class="content">
			<u-button type="primary" @click="upData">提交反馈</u-button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				action: 'http://www.example.com/upload',
				filesArr: [],
				txt: '',
				mobile: '',
				list: [{
						name: 'BUG',
						disabled: false,
						type: 1
					},
					{
						name: '产品建议',
						disabled: false,
						type: 2
					},
					{
						name: '吐槽',
						disabled: false,
						type: 3
					},
					{
						name: '其他',
						disabled: false,
						type: 4
					}
				],
				value: 'BUG'
			};
		},
		methods: {
			beforeUpload(index, list) {
				// 只上传偶数索引的文件
				let that = this
				uni.uploadFile({
					url: 'http://47.99.121.209:8093/file/single', // 后端api接口
					filePath: list[0].url, // uni.chooseImage函数调用后获取的本地文件路劲
					sizeType: 'compressed',
					formData: {
						dir: 'static/img'
					},
					name: 'file', //后端通过'file'获取上传的文件对象
					header: {
						Authorization: `Bearer` + 'eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJzdHU5NTc3IiwiY3JlYXRlZCI6MTYwNDI5NTcxNTY4MSwiZXhwIjoxNjA0OTAwNTE1fQ.SXm4oSIkJpfeP1t0gPJWthbU1_EjoI9_Km_DLfQOlsNzopDa_WIAwifT0w2tbKb9zETUvCGJgcYeEb5HnVOkbg'
					},
					success: (res) => {		
						that.icon = JSON.parse(res.data).data;
						this.filesArr.push(that.icon);
					}
				})
			},
			upData() {
				if (this.txt == '') {
					uni.showToast({
						icon: 'none',
						title: '反馈内容不能为空！'
					});
				}
				if ((/^1[23456789]\d{9}$/.test(this.mobile)) != true) {
					uni.showToast({
						icon: 'none',
						title: '请输入正确手机号！'
					});
				} else {
					this.$refs.uUpload.upload();
					this.$u.vuex('vuex_token',
						"eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJzdHU5NTc3IiwiY3JlYXRlZCI6MTYwNDI5NTcxNTY4MSwiZXhwIjoxNjA0OTAwNTE1fQ.SXm4oSIkJpfeP1t0gPJWthbU1_EjoI9_Km_DLfQOlsNzopDa_WIAwifT0w2tbKb9zETUvCGJgcYeEb5HnVOkbg"
					)
					this.$u.post('http://47.99.121.209:8093/feedback/myfeedback', {
						feedbackTypeId: this.value,
						feedbackImg: this.filesArr,
						feedbackDetails: this.txt,
						phone: this.mobile
					}).then(res => {
						uni.showToast({
							title: "反馈成功",
							duration: 2000,
							success() {
								setTimeout(function() {
									uni.navigateTo({
									    url: '../wode/wode'
									});
								}, 1000);
							}
						});
					})
				}
			},
			goHistory() {
				uni.navigateTo({
				    url: '../history-feedback/history-feedback'
				});
			}
		},
	};
</script>
</script>

<style>
	.box {
		width: 100vw;
		height: 100wh;
		background-color: #F1F1F1;
	}

	.content {
		margin-bottom: 30rpx;
		background-color: #FFFFFF;
	}

	.p1 {
		margin-bottom: 30rpx;
		position: relative;
		left: 80%;
	}

	.text {
		position: relative;
		top: 20rpx;
		margin-bottom: 40rpx;
		/* 		font-family: 'PingFangSC-Regular';
		font-weight: 'Regular';
		font-size: 20sp;
		color: #333333; */
	}
</style>
