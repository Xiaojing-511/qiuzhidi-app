<template>
	<view class="content">
		<view class="fgt-group">
			<view class="top_text">
				<text>使用注册的手机号找回密码</text>
			</view>
			<view class="box">
				<view class="img_box">
					<image class="fgt_img" src="../../static/img/phone.png" mode=""></image>
				</view>
				<input class="fgt-input u-m-t-50 u-p-b-20" type="number" v-model="phone" placeholder="请输入手机号" :placeholder-style="input_style" />
			</view>
			<view class="box" >
				<view class="img_box">
					<image class="fgt_img" src="../../static/img/3.png" mode=""></image>
				</view>
				<input class="fgt-input u-m-t-50 u-p-b-20" type="number" v-model="code" placeholder="请输入验证码" :placeholder-style="input_style" />
				<view class="getCode_btn">
					<button class="getCode" v-show="isShow" @click="getCode" >获取验证码</button>
					<button class="getCode" v-show="!isShow" disabled="true" >还有{{timer}}秒</button>
			<!-- 		<a class="getCode" v-show="isShow" @click="getCode" >获取验证码</a>
					<a class="getCode" v-show="!isShow" disabled="true" >还有{{timer}}秒</a> -->
				</view>
			</view>
			<view class="box">
				<view class="img_box">
					<image class="fgt_img" src="../../static/img/9.png" mode=""></image>
				</view>
				
				<input class="fgt-input u-m-t-50 u-p-b-20" type="password" v-model="password" placeholder="请输入新密码"
				 :placeholder-style="input_style" />

			</view>
			<view class="box">
				<view class="img_box">
					<image class="fgt_img" src="../../static/img/9.png" mode=""></image>
				</view>
				
				<input class="fgt-input u-m-t-50 u-p-b-20" type="password" v-model="repassword" placeholder="请确认新密码"
				 :placeholder-style="input_style" />

			</view>
			<u-button @click="changeps" :custom-style="customStyle_Push">注册</u-button>
		</view>
		<u-toast ref="uToast" />
	</view>
</template>

<script>
	export default {
		data() {
			return {
				phone: '',
				timer: 60,
				isShow: true,
				code: '',
				input_style: 'color:#999999;font-size:30rpx;',

					
					
				
					

				// },

				customStyle_Push: {
					width: '600rpx',
					height: '90rpx',
					margin: '100rpx auto',
					color: '#ffffff',
					fontSize: '36rpx',
					background: '#00A8FF'
				}
			}
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
						this.showToast('发送成功', 'success')
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
			changeps() {
				console.log(!this.$u.test.isEmpty(this.password));
				if (!this.$u.test.mobile(this.phone)) {
					this.showToast('请输入正确的手机号')
				} else if (!this.password || !this.repassword) {
					this.showToast('请输入密码')
				} else if (!this.$u.test.enOrNum(this.password)) {
					this.showToast('密码只能由字母和数字组成')
				} else if (!this.$u.test.rangeLength(this.password, [6, 20])) {
					this.showToast('密码长度要在6-20位之间')
				} else if (this.password != this.repassword) {
					this.showToast('两次密码不一致')
				} else if (!this.$u.test.code(this.code, 6)) {
					this.showToast('请输入正确格式的验证码')
				}
				//else if(){
				//此部分为验证码是否与短信的匹配
				// }
				else {
					this.$u.post('/sso/updatePassword', {
						telephone: this.phone,
						password: this.password,
						authCode: this.code

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
	.content {
		background-color: #EEEEEE;
		width: 100%;
		height: 100%;
		position: absolute;
	}

	.fgt-input {
		padding-left:20rpx ;
		padding-top: 2rpx;
	}
	
	.getCode_btn{
		position: absolute;
		right: 100rpx;
		top: 340rpx;
		
		border-left:1rpx solid rgba($color: #00A8FF, $alpha: 1.0) ;
		height: 70rpx;
		
	
	}
	// .getCode_btn a {
	// 	height: 100%;
	// 	line-height: 100%;
	// 	width: 100%;
	// 	margin-left: 20rpx;
	// 	margin-top:20rpx ;
		
	// }
	
	
	.img_box{
		background-color: #FFFFFF;
		width: 100rpx;
		height: 110rpx;
		margin-top:50rpx ;
		border-right:1rpx solid rgba($color: #000000, $alpha: 0.5) ;
	
	}

	.top_text text {

		color: #999999;
		font-size: 24rpx;
	}

	.fgt_img {
		width: 40rpx;
		height: 64rpx;
		background-color: #FFFFFF;
		display: block;
		margin: 20rpx auto;
		line-height: 64rpx;
		
	}

	.fgt-group {
		display: block;
		width: 600rpx;
		margin: 80rpx auto;
		
	}
	
	.getCode {
		
			
			color: #00A8FF;
			font-size: 30rpx;
			background: #FFFFFF;
			margin-right: -20rpx;
			margin-left:4rpx ;
			border: 0;
			outline: none;
			
			
	}
	.getCode::after {
		border: none;
	}
	
	
	

	.box {
		display: flex;
		
	}

	.fgt-input {
		width: 600rpx;
		height: 90rpx;
		background: #FFFFFF;
		border-radius: 5rpx;
	}
</style>
