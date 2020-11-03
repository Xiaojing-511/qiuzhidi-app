<template>
	<view class="contect">
		<view class="box" v-for="(item,index) in list">
			<view class="history-feedback-view border-bottom">
				<view class="verify-left">
					反馈意见
				</view>
				<view class="verify-middle">
					<view class="choose" v-if="item.feedbackTypeId==1">
						bug
					</view>
					<view class="choose" v-if="item.feedbackTypeId==2">
						产品意见
					</view>
					<view class="choose" v-if="item.feedbackTypeId==3">
						吐槽
					</view>
					<view class="choose" v-if="item.feedbackTypeId==4">
						其他
					</view>
				
				</view>
				<view class="verify-right">
					提交日期:{{item.createTime.substring(0,10)}}
				</view>
			</view>
			<view class="history-feedback-view feedback-picture" style="display: flex;flex-wrap: wrap;">
				<view class="image-text">
					{{item.feedbackDetails}}
				</view>
				<view class="image-box"  v-for="i in item.feedbackImg.split(',')">
				<image class="image" :src="'http://47.99.121.209:8093/' + i " mode="aspectFit" ></image>
			</view>

			</view>
			<view class="history-feedback-view service-feedback">
				客服反馈：sorry,感谢您提出的问题，正在。。
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				feedbacktype: '',
				data: '',
				opinion: '我是反馈的信息',
				list:'[]',
				
			}
		},
		onLoad() {
			console.log(666666666);
			this.$u.vuex('vuex_token',
				"eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJzdHU5NTc3IiwiY3JlYXRlZCI6MTYwNDI5NTcxNTY4MSwiZXhwIjoxNjA0OTAwNTE1fQ.SXm4oSIkJpfeP1t0gPJWthbU1_EjoI9_Km_DLfQOlsNzopDa_WIAwifT0w2tbKb9zETUvCGJgcYeEb5HnVOkbg"
			)
			this.$u.post('http://47.99.121.209:8093/feedback/historyFeedback', {

			}).then(res => {
				// 将token存储在vuex中（用的uview优化过的写法，vuex_token字段配置了，可以直接存在了本地存储中）
				// 返回原本所在页面

				console.log(this.$u);
				console.log(res)
				console.log(res[0].feedbackTypeId)
				this.list=res;
				console.log(this.list)
				console.log(this.list[0].feedbackImg)
				console.log(this.list[0].feedbackImg.split(","))
			})
		},
		methods: {

		}
	}
</script>

<style>
	.history-feedback-view {
		display: flex;
		/* flex-wrap: wrap; */
		font-family: PingFangSC-Regular;
		padding-top: 20px;
	}

	.box {
		background-color: #FFFFFF;
		margin-top: 12px;
		width: 100%;
		padding-left: 20px;
		padding-right: 20px;
	}

	.verify-left {
		display: inline-block;
		padding-bottom: 20px;
		padding-right: 13.33px;
		font-size: 20px;
	}

	.choose {
		display: inline-block;
		background: #00A8FF;
		border-radius: 3.33px;
		width: 40px;
		/* height: auto!important;
		height: 26.67px;
		min-height: 26.67px; */
		font-size: 16px;
		text-align: center;
		color: #FFFFFF;
		margin: 5px 0px;
	}

	.verify-right {
		display: inline-block;
		font-size: 16px;
		color: #999999;
		position: absolute;
		right: 0px;
		padding-right: 20px;
	}

	.feedback-picture {
		border-bottom: 1px solid #DDDDDD;
		padding-bottom: 20px;
		display: flex;
		flex-wrap: wrap;
	}



	.image-text {
		color: #999999;
		font-size: 16px;
		width:100%;
height:30px	
}

	.contect {
		background: #EEEEEE;
		width: 100%
	}

	.border-bottom {
		border-bottom: 1px solid #DDDDDD;
	}

	.service-feedback {
		padding-bottom: 70px;
		font-size: 16px;
		color: #999999;
	}

</style>
