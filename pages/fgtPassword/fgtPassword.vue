<template>
	<view class="content">
		<view class="fgt-group">
			<view class="u-border-bottom">
				<input class="fgt-input u-m-t-50 u-p-b-20" type="number" v-model="phone" placeholder="请输入手机号" :placeholder-style="input_style" />
			</view>
			<u-button v-show="isShow" @click="getCode" :custom-style="customStyle">获取验证码</u-button>
			<u-button v-show="!isShow" disabled="true" :custom-style="customStyle">还有{{timer}}秒</u-button>
			<view>
				<input class="fgt-input u-m-t-50 u-p-b-20" type="number" v-model="code" placeholder="请输入验证码" :placeholder-style="input_style" />
			</view>
			<view>
				<input class="fgt-input u-m-t-50 u-p-b-20" type="password" v-model="password" placeholder="请输入新密码"
				 :placeholder-style="input_style" />
			</view>
			<view>
				<input class="fgt-input u-m-t-50 u-p-b-20" type="repassword" v-model="repassword" placeholder="请确认新密码"
				 :placeholder-style="input_style" />
			</view>
				<u-button  @click="changeps" :custom-style="customStyle">提交</u-button>
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
				isShow: true
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
				} else if(this.password!=this.repassword){
					this.showToast('两次密码不一致')
				}else if (!this.$u.test.code(this.code, 6)) {
					this.showToast('请输入正确格式的验证码')
				}
				//else if(){
				//此部分为验证码是否与短信的匹配
				// }
				else {
					this.$u.post('/sso/easyUpdatePassword/'+this.phone, {
						id: this.phone, 
						password: this.password, 
						
					}).then((res) => {
						console.log(res)
						uni.navigateTo()
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

<style lang="scss">
	.content {
		background: $color_emphasize;
		width: 100%;
	}
</style>
