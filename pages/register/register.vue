<template>
	<view class="content">
		<view class="register-group">
			<view class="u-border-bottom code-box">
				<input class="register-input u-m-t-50 u-p-b-20" type="number" v-model="phone" placeholder="请输入手机号"
				 :placeholder-style="input_style" />
				<u-button v-show="isShow" @click="getCode" :custom-style="customStyle_GetCode">获取验证码</u-button>
				<u-button v-show="!isShow" disabled="true" :custom-style="customStyle_GetCode">还有{{timer}}秒</u-button>
			</view>
			<view class="u-border-bottom">
				<input class="register-input u-m-t-50 u-p-b-20" type="password" v-model="password" placeholder="请输入密码"
				 :placeholder-style="input_style" />
			</view>
			<view class="u-border-bottom">
				<input class="register-input u-m-t-50 u-p-b-20" type="password" v-model="repassword" placeholder="请再次输入密码"
				 :placeholder-style="input_style" />
			</view>
			<view class="u-border-bottom">
				<input class="register-input u-m-t-50 u-p-b-20" type="number" v-model="code" placeholder="请输入验证码"
				 :placeholder-style="input_style" />
			</view>
			<u-button @click="regist" :custom-style="customStyle" shape="circle">注册</u-button>
		</view>
		<u-toast ref="uToast" />
	</view>
</template>
<script>
	export default {
		data() {
			return {
				phone: '',
				password: '',
				repassword: '',
				code: '',
				timer: 60,
				isShow: true,
				// 注意该属性的值只支持字符串形式
				input_style: 'color:#FFFFFF;font-size:30rpx;',
				// 必须这么写，不然在小程序中不生效
				customStyle: {
					width: '200rpx',
					height: '80rpx',
					margin: '60rpx auto',
					color: '#00A8FF',
					fontSize: '36rpx',
					background: '#ffffff'
				},
				customStyle_GetCode:{
					width: '200rpx',
					height: '80rpx',
					color: '#FFFFFF',
					fontSize: '30rpx',
					background: '#00A8FF',
					marginTop:'32rpx',
					marginBottom:'20rpx'
					
				},
			}
		},
		computed: {

		},
		methods: {
			getCode() {
				if (!this.$u.test.mobile(this.phone)) {
					this.showToast('请输入正确的手机号')
				} else {
					this.$u.get('/sso/getAuthCode', {
						telephone: this.phone
					}).then((res) => {
						console.log(res);
						this.isShow = false;
						var times = setInterval(() => {
							this.timer--;
							if (this.timer < 1) {
								this.isShow = true;
								this.timer = 60;
								clearInterval(times)
							}
						}, 1000)
					}).catch((res) => {
						console.log(res)
						this.showToast(res.message, 'error')
					})
				}
			},
			regist() {
				console.log(!this.$u.test.isEmpty(this.password));
				if (!this.$u.test.mobile(this.phone)) {
					this.showToast('请输入正确的手机号')
				} else if (!this.password || !this.repassword) {
					this.showToast('请输入密码')
				} else if (!this.$u.test.enOrNum(this.password)) {
					this.showToast('密码只能由字母和数字组成')
				} else if (!this.$u.test.rangeLength(this.password, [6, 20])) {
					this.showToast('密码长度要在6-20位之间')
				} else if(this.password!=this.repassword){
					this.showToast('两次密码不一致')
				}else if (!this.$u.test.code(this.code, 6)) {
					this.showToast('请输入正确格式的验证码')
				}
				//else if(){
				//此部分为验证码是否与短信的匹配
				// }
				else {
					this.$u.post('/sso/register', {
						telephone: this.phone, //17090888281
						password: this.password, //123456
						authCode:this.code
					}).then((res) => {
						console.log(res)
						uni.navigateTo({
									url: '../login/login'
							 	})
					}).catch(res => {
						console.log(res)
						this.showToast(res.message, 'error')
					})
				}
			},
			showToast(msg, type = 'warning') {
				this.$refs.uToast.show({
					title: msg,
					type: type,
					position: 'top'
				})
			}
		}
	}
</script>
<style lang="scss" scoped>
	.code-box {
		display: flex;
	}

	.content {
		background: $color_emphasize;
		width: 100%;

		.register-group {
			display: block;
			width: 500rpx;
			margin: 250rpx auto;

			.register-input {
				color: #FFFFFF;
				font-size: $font_general_big;
			}


		}

	}
</style>
