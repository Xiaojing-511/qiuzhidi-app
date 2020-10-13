<template>
	<view class="content">
		<navigator url="../login/login">登录/注册</navigator>
		<navigator url="../login/code">验证码</navigator>
		<view class="u-font-40 u-tips-color">昵称:{{nickname}} </view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				nickname: '',
				username: '',
				id: 0
			}
		},
		onLoad() {
			console.log(this.vuex_token);
			this.getInfo();
		},
		methods: {
			getInfo() {
				this.$u.get('member/info/getMemberInfo', {
					username: 'test'
				}).then(res => {
					["nickname", "username", "id"].forEach((key) => {
						if (key in res) {
							this[key] = res[key]
						}
					});
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.content {}
</style>
