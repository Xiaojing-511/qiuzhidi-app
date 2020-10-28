<template>
	<view class="center u-flex-wrap">
		<view class="top ">
			<view class="input-box u-m-l-50 u-flex">
				<u-icon class="u-m-l-30" name="search" color="#999999" size="28"></u-icon>
				<input class="select-input uni-input u-p-l-15 " confirm-type="search" type="text" placeholder="JAVA工作" :placeholder-style="inputStyle" />
			</view>
		</view>
		<scroll-view scroll-y="true" class="massage-box u-flex-wrap">
			<view class="interplation-msg  u-m-t-20 u-flex-wrap" v-for="(item, index) in msgList" :key="item.interpolateId">
				<view class="u-flex">
					<view class="company">{{ item.companies }}</view>
					<view class="salary">{{ item.salary }}元</view>
				</view>
				<view class="u-flex">
					<image class="head-img" src="../../static/img/qq.png" mode="widthFix"></image>
					<view class="u-flex-wrap">
						<view class="u-flex">
							<view class="nickname u-m-l-30 ">昵称</view>
							<view class="position  ">{{ item.position }}</view>
						</view>
						<view class="resume u-m-t-20 u-m-l-30 ">学校</view>
						<view class="through u-m-t-18  ">浏览：1200次</view>
					</view>
				</view>
			</view>
		</scroll-view>
		<view class="buttom u-p-b-192  "><button @click="toIssue" :custom_style="customStyle" class="issue u-m-l-75 " style="">我要内推</button></view>
		<u-toast ref="uToast" />
	</view>
</template>

<script>
export default {
	data() {
		return {
			msgList: [],
			inputStyle: 'background: #F2F2F2;',
			customStyle: {
				backgroundColor: '#00A8FF',
				color: '#FFFFFF'
			}
		};
	},
	onLoad() {
		this.getMassage();
		// console.log(this.msgList);
	},
	methods: {
		getMassage() {
			console.log('获取信息');
			this.$u
				.get('/ipl/list', {})
				.then(res => {
					// console.log(res);
					// console.log(this);
					// var textList = [];
					// this.msgList = res.umsInterpolate;

					for (var i = 0; i < res.length; i++) {
						// this.textList[i] = res[i].umsInterpolate;
						this.msgList[i] = res[i].umsInterpolate;
					}
					// this.msgList = textList;
					console.log(this.msgList);
				})
				.catch(err => {
					console.log(err);
				});
		},

		toIssue() {
			this.$u
				.post('/ipl/info', {})
				.then(res => {
					console.log(res);
					var n = res;
					if (n !== '成功登录') {
						this.showToast('请先去登陆', 'warning');
					} else {
						uni.navigateTo({
							url: '../issueInterpolation/issueInterpolation'
						});
					}
				})
				.catch(err => {
					this.showToast('请先去登陆', 'warning');
				});
		},

		showToast(massage, toastType, navTo) {
			this.$refs.uToast.show({
				title: massage,
				type: toastType,
				isTab: true,
				url: navTo,
				icon: false
			});
		}
	}
};
</script>

<style lang="scss">
body {
	padding: 0;
	margin: 0;
}
page{
	background-color: #EEEEEE;
}
.center {
	width: 100%;
}
.top {
	height: 80rpx;
	// z-index: 2;
	background-color: #ffffff;
	align-self: flex-start;
}
.input-box {
	width: 650rpx;
	height: 64rpx;
	background-color: #f2f2f2;
	border-radius: 32px;
}
.select-input {
	width: 550rpx;
	height: 64rpx;
	font-size: 24rpx;
	line-height: 24rpx;
}
.interplation-msg {
	background-color: #FFFFFF;
	width: 750rpx;
	height: 244rpx;
	overflow: hidden;
	// z-index: 1;
}
.massage-box{
	height: 1000rpx;
}
.head-img {
	width: 120rpx;
	height: 120rpx;
}
.company {
	width: 625rpx;
	margin-top: 30rpx;
	padding-left: 32rpx;
	font-size: 36rpx;
	font-weight: Semibold;
	color: #333333;
}
.salary {
	margin-top: 30rpx;
	font-size: 30rpx;
	color: #00a8ff;
}
.head-img {
	margin-left: 30rpx;
}
.nickname {
	width: 137rpx;
	font-size: 30rpx;
	font-weight: Medium;
}
.position {
	font-size: 24rpx;
	color: #999999;
}
.through {
	margin-left: 33rpx;
	font-size: 24rpx;
	color: #999999;
}
.resume {
	font-size: 24rpx;
}
.issue {
	width: 600rpx;
	height: 90rpx;
	background-color: #00a8ff;
	color: #ffffff;
	font-size: 30rpx;
	// margin-top: 40rpx;
}
.buttom {
	margin-top: 20rpx;
	width: 100%;
	height: 140rpx;
	display: block;
	align-self: flex-end;
	// padding-bottom: 40rpx;
	// bottom: 0;
	// z-index: 2;
	// position: fixed;
	// background-color: #ffffff;
}
</style>
