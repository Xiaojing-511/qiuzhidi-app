<template>
	<view class="box">
		<view >
			<p class="p1" @click="goHistory">历史反馈</p>
		</view>
		<view class="content">
			<view class="text">反馈类型</view>
			<u-line color="#c8c9cc"></u-line>
			<u-radio-group v-model="value" @change="radioGroupChange">
				<u-radio @change="radioChange" v-for="(item, index) in list" :key="index" :name="item.name" :disabled="item.disabled">
					{{item.name}}
				</u-radio>
			</u-radio-group>
		</view>
		
		
		<view class="content">
			<view class="text">反馈意见</view>
			<u-line color="#c8c9cc"></u-line>
			<u-field v-model="txt" label=" " placeholder="您得反馈对我们很重要,请在此输入." type="textarea">
			</u-field>
			<u-upload ref="uUpload" max-count="6"></u-upload>
		</view>
		
		
		<view class="content">
			<view class="text">联系电话</view>
			<u-line color="#c8c9cc"></u-line>
			<u-field v-model="mobile" label=" " placeholder="留下你的电话方便我们联系你哦~">
			</u-field>
		</view>
		
		
		<view class="content">
			<u-button type="primary" @click="upData">提交反馈</u-button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				filesArr: [],
				txt:'',
				mobile: '',
				list: [{
						name: 'BUG',
						disabled: false
					},
					{
						name: '产品建议',
						disabled: false
					},
					{
						name: '吐槽',
						disabled: false
					},
					{
						name: '其他',
						disabled: false
					}
				],
				value: 'BUG'
			};
		},
		methods: {
			upData(){
				// this.filesArr = this.$refs.uUpload.lists;
				// console.log(this.filesArr);
				// console.log(this.value);
				// console.log(this.txt);
				// console.log(this.mobile);
				this.$u.post('http://www.example.com', {
								
							}).then(res => {
								console.log(res+'sucsess');
							});
			},
			goHistory(){
				this.$u.route('pages/history-feedback/history-feedback');
			}
		},
	};
</script>
</script>

<style>
	.box{
		background-color:#F1F1F1;
		width: 100vw;
		height: 100vh;
	}
	.content{
		margin-bottom: 30rpx;
		background-color: #FFFFFF;
	}
	.p1{
		margin-bottom: 30rpx;
		position: relative;
		left: 80%;
	}
	.text{
		position: relative;
		top: 20rpx;
		margin-bottom: 40rpx;
		font-family: 'PingFangSC-Regular';
		font-weight: 'Regular';
		font-size: 20sp;
		color: #333333;
	}
</style>
