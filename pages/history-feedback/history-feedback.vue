<template>
	<view class="contect">
		<view class="box">
		<view class="history-feedback-view border-bottom">
			<view class="verify-left">
				反馈意见
			</view>
			<view class="verify-middle">
				{{feedbacktype}}
			</view>
			<view class="verify-right">
				提交日期:{{data}}
			</view>
		</view>
		<view class="history-feedback-view feedback-picture">
			<view class="image-text">
				{{opinion}}
			</view>
			<view class="image-box" v-for="(item) in list">
				<image src="item" mode="aspectFit" style="width: 50px;height: 50px;"></image>
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
				feedbacktype:'bug',
				data:'2020.09.10',
				opinion:'我是反馈的信息',
				list:'',
			}
		},
		onLoad() {
		this.$u.get(' ',{
			
		}).then(res=>{
			console.log(res);
		})
		},
		methods: {
			
		}
	}
</script>

<style>
.history-feedback-view{
	display: flex;
	font-family: PingFangSC-Regular;
	padding-top: 20px;
}
.box{
	background-color:#FFFFFF;
	margin-top: 12px;
	width: 100%;
	padding-left: 20px;
	padding-right: 20px;
}
.verify-left{
	display: inline-block;
	padding-bottom: 20px;
	padding-right: 13.33px;
	font-size:20px;
}
.verify-middle{
	display: inline-block;
	background:#00A8FF;
	border-radius: 3.33px;
	width: 40px;
	height: 26.67px;
	font-size: 16px;
	text-align: center;
	color: #FFFFFF;
}
.verify-right{
	display: inline-block;
	font-size: 16px;
	color: #999999;
	position: absolute;
	right: 0px;
	padding-right: 20px;
}
.feedback-picture{
	border-bottom:1px solid #DDDDDD;
	padding-bottom: 20px;
}
.image-box{
	
}
.image-text{
	color:#999999;
	font-size:16px
}
.contect{
	background:#EEEEEE;
	width:100%
}
.border-bottom{
	border-bottom:1px solid #DDDDDD;
}
.service-feedback{
	padding-bottom: 70px;
	font-size: 16px;
	color: #999999;
}
</style>
