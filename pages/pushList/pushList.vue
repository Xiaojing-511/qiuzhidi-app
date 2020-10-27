<template>
	<view>
		<view class="top">
			<view class="nav u-flex">
				<navigator class="nav-item   u-m-20 u-m-l-60 u-p-b-20 u-p-t-20 u-border-bottom" url="">全部</navigator>
				<navigator class="nav-item   u-m-20 u-p-b-22 u-p-t-20" url="">未响应</navigator>
				<navigator class="nav-item   u-m-20 u-p-b-22 u-p-t-20" url="">待确定</navigator>
				<navigator class="nav-item   u-m-20 u-p-b-22 u-p-t-20" url="">进行中</navigator>
				<navigator class="nav-item   u-m-20 u-p-b-22 u-p-t-20" url="">成功/失败</navigator>
			</view>
		</view>

		<view class="msg-list">
			<view class="pushMsg u-flex-wrap u-col-top u-m-l-35" v-for="(item, index) in myIssueList" :key="item.interpolateState">
				<view class="u-flex">
					<view class="need u-m-l-34 u-m-t-12">求{{ item.companies }}内推</view>
					<view class="need u-m-t-32 u-m-l-540">{{ item.salary }}元</view>
				</view>
				<view class="u-flex u-m-t-40">
					<image class="head-img  u-m-l-34 u-m-r-34" :src="imgUrl" mode="widthFix"></image>
					<view class="need  u-m-l-124 u-m-r-32">d</view>
					<view class="need  u-m-l-306">s</view>
				</view>
				<view class="u-flex u-m-t-18">
					<view class="need u-m-t-198 u-m-l-32">发布时间：{{ item.issueTime }}</view>
					<view class="need u-m-t-202 u-m-l-428">响应人数：{{ item.interpolateId }}</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			myIssueList: []
		};
	},
	onLoad() {
		this.getList();
	},
	methods: {
		getList() {
			this.$u
				.get('/ipl/mylist', {})
				.then(res => {
					console.log(res);
					console.log(res[0].umsInterpolates);
					for (var i = 0; i < res[0].umsInterpolates.length; i++) {
						this.myIssueList[i] = res[0].umsInterpolates[i];
					}
					console.log(this.myIssueList);
				})
				.catch(err => {
					console.log(err);
				});
		}
	}
};
</script>

<style lang="scss">
page {
	padding: 0;
	margin: 0;
}
.top {
	width: 100%;
	position: fixed;
	background-color: #ffffff;
	z-index: 2;
}
.nav {
	width: 750rpx;
}
.nav-item {
	align-items: center;
	text-align: center;
	box-sizing: border-box;
}
.u-border-bottom {
	border-bottom: 1px solid #000000;
}
.msg-list {
	width: 750rpx;
}
.pushMsg {
	width: 680rpx;
	height: 252rpx;
	background-color: #efeaea;
	margin-top: 152rpx;
	align-content: flex-start;
	justify-content: flex-start;
}
.need {
	flex-grow: 1;
	font-size: 15px;
}
.head-img {
	width: 60rpx;
	height: 60rpx;
	border-radius: 50%;
	z-index: 1;
}
</style>
