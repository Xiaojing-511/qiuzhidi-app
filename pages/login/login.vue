<template>
	<view class="content">
		<view class="login-group">
			<view class="u-border-bottom">
				<input class="login-input u-m-t-50 u-p-b-20" type="number" v-model="phone" placeholder="请输入手机号" :placeholder-style="input_style" />
			</view>
			<view class="u-border-bottom">
				<input class="login-input u-m-t-50 u-p-b-20" type="password" v-model="password" placeholder="请输入密码"
				 :placeholder-style="input_style" />
			</view>
			<view class=" u-flex u-row-between  u-m-t-20 ">
				<navigator class="link " url="">忘记密码</navigator>
				<navigator class="link " url="../switchIdentity/switchIdentity">去注册</navigator>
			</view>
			<u-button @click="login" :custom-style="customStyle" shape="circle">登录</u-button>
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
				}
			}
		},
		computed: {

		},
		methods: {
			login() {
				console.log(!this.$u.test.isEmpty(this.password));
				if (!this.$u.test.mobile(this.phone)) {
					this.showToast('请输入正确的手机号')
				} else if (!this.password) {
					this.showToast('请输入密码')
				} else if (!this.$u.test.enOrNum(this.password)) {
					this.showToast('密码只能由字母和数字组成')
				} else if (!this.$u.test.rangeLength(this.password, [6, 20])) {
					this.showToast('密码长度要在6-20位之间')
				} else {
					this.$u.post('sso/login', {
						phone: this.phone, //17090888281
						password: this.password, //123456
					}).then(res => {
						// 将token存储在vuex中（用的uview优化过的写法，vuex_token字段配置了，可以直接存在了本地存储中）
						this.$u.vuex('vuex_token', res.token)
						// 返回原本所在页面
						uni.navigateBack()
					}).catch(res => {
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
	.content {
		background: $color_emphasize;
		width: 100%;

		.login-group {
			display: block;
			width: 500rpx;
			margin: 250rpx auto;

			.login-input {
				color: #FFFFFF;
				font-size: $font_general_big;
			}

			.link {
				color: white;
				font-size: $font_general_small;
			}
		}

	}
</style>
