<template>
	<view class="content">
		<view class="iner_box">

			<view class="first_box">
				<view class="tip_text">
					<text>基本信息</text>
				</view>
				<view class="msg">
					
					<view class="tip">
						昵称:<input type="text" v-model="name" value="" />
					</view>
					
					<view class="tip">
						职位:<input type="text" v-model="place" value="" />
					</view>
					
					<view class="tip">
						工龄:<input type="text" v-model="age" value="" />
					</view>
					
					<view class="tip">
						公司:<input type="text" v-model="company" value="" />
					</view>
					
					<view class="tip_img">
						评分:<image class="img_star" src="" mode=""></image>
						<image class="img_star" src=""  mode=""></image>
					</view>
					
					
				</view>
			</view>

			<view class="second_box">
				<view class="tip_text">
					<text>工作经历</text>
					<input type="text" v-model="story" value="" />
				</view>



			</view>

			<view class="third_box">
				<view class="tip_text">
					<text>面试侧重点</text>
					<view class="tip_text_iner">
						
					</view>
				</view>
			</view>


		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				name:"",
				place:"",
				age:"",
				company:"",
				story:''
			}
		},
		methods: {

		}
	}
</script>

<style lang="scss" scoped>
	.contect {
		height: 100%;
		width: 100%;
	}

	.iner_box {
		display: block;
		width: 600rpx;
		margin: 80rpx 40rpx;

	}

	.tip_text {
		font-size: 36rpx;

		font-family: PingFangSC;
		font-weight: 600;
		color: #333333;

	}

	.msg {
		margin-top: 60rpx;
		margin-left: 60rpx;
		
}
	.msg text {
		display: flex;
	}
	
	.tip{
		display: flex;
		margin-bottom: 20rpx;
		
		font-size: 30rpx;
		font-family: PingFang;
		font-weight: 500;
		color: #333333;
	}
</style>
