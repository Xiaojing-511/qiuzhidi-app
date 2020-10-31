<template>
	<view class="content">
		<view class="search-box">
			<u-search shape="round" :clearabled="false" :animation="false" :show-action="false" height="64" placeholder="请输入工作或公司"
			 v-model="keyword" bg-color='#F2F2F2' @search="search"></u-search>
		</view>
		<view class="keyword-box">
			<view class="keyword-block" v-if="oldKeywordList.length>0">
				<view class="keyword-list-header">
					<view>历史搜索</view>
					<view class="last-list" @click="oldDelete()">清空历史记录</view>
				</view>
				<view class="keyword">
					<view v-for="(item,index) in oldKeywordList" @tap="doSearch(item)" :key="index">{{item}}</view>
				</view>
			</view>
			<view class="keyword-block" v-if="hotKeywordList.length>0">
				<view class="keyword-list-header">
					<view>热门搜索</view>
				</view>
				<view class="keyword">
					<view v-for="(keyword,index) in hotKeywordList" @tap="doSearch(keyword)" :key="index">{{keyword}}</view>
				</view>
			</view>

		</view>
	</view>
</template>

<script>
	export default {
		onLoad(option) { //option为object类型，会序列化上个页面传递的参数
			console.log(option.name); //打印出上个页面传递的参数。
			this.keyword = option.name
			this.loadOldKeyword()
			this.loadHotlist()
		},
		data() {
			return {
				keyword: '',
				oldKeywordList: [],
				hotKeywordList: [],
			}
		},
		methods: {
			search() {
				this.doSearch(this.keyword)
			},
			doSearch(keyword) {
				uni.navigateTo({
					url: '../search/search?name=' + keyword
				})
				this.saveKeyword(keyword)
			},
			//加载历史记录
			loadOldKeyword() {
				uni.getStorage({
					key: 'OldKeys',
					success: (res) => {
						var OldKeys = JSON.parse(res.data);
						this.oldKeywordList = OldKeys;
					}
				});
			},
			//清空搜索记录
			oldDelete() {
				uni.showModal({
					content: '确定清除历史搜索记录？',
					success: (res) => {
						if (res.confirm) {
							console.log('用户点击确定');
							this.oldKeywordList = [];
							uni.removeStorage({
								key: 'OldKeys'
							});
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			},
			loadHotlist() {
				this.hotKeywordList = ['11', '222', '3333']
			},
			//保存搜索历史的方法
			saveKeyword(keyword) {
				uni.getStorage({
					key: 'OldKeys',
					success: (res) => {
						var OldKeys = JSON.parse(res.data);
						var findIndex = OldKeys.indexOf(keyword);
						if (findIndex == -1) {
							OldKeys.unshift(keyword);
						} else {
							OldKeys.splice(findIndex, 1);
							OldKeys.unshift(keyword);
						}
						//最多10个纪录
						OldKeys.length > 10 && OldKeys.pop();
						uni.setStorage({
							key: 'OldKeys',
							data: JSON.stringify(OldKeys)
						});
						this.oldKeywordList = OldKeys; //更新历史搜索
					},
					fail: (e) => {
						var OldKeys = [keyword];
						uni.setStorage({
							key: 'OldKeys',
							data: JSON.stringify(OldKeys)
						});
						this.oldKeywordList = OldKeys; //更新历史搜索
					}
				});
			}
		}
	}
</script>

<style>
	.page {
		width: 100%;
	}

	.search-box {
		width: 100%;
	}

	.content {
		width: 100%;
		padding: 0 30rpx;
	}

	.keyword-box {
		height: calc(100vh - 110rpx);
		border-radius: 20rpx 20rpx 0 0;
		background-color: #fff;
	}

	.keyword-box .keyword-block {
		padding: 10rpx 0;
	}

	.keyword-box .keyword-block .keyword-list-header {
		width: 100%;
		padding: 10rpx;
		font-size: 27rpx;
		color: #333;
		display: flex;
		justify-content: space-between;
	}

	.keyword-box .keyword-block .keyword {
		width: 94%;
		padding: 3px 3%;
		display: flex;
		flex-flow: wrap;
		justify-content: flex-start;
	}

	.keyword-box .keyword-block .keyword>view {
		display: flex;
		justify-content: center;
		align-items: center;
		border-radius: 60rpx;
		padding: 0 20rpx;
		margin: 10rpx 20rpx 10rpx 0;
		height: 60rpx;
		font-size: 28rpx;
		background-color: rgb(242, 242, 242);
		color: #6b6b6b;
	}
</style>
