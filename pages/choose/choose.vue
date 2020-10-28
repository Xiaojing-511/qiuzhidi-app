<template>
	<view class="center ">
		<view class="box">
			<view class="u-flex">
				<text class="text-area u-m-l-22 u-m-t-28 u-m-r-52">公司：</text>
				<input type="text" class="company u-m-t-26" />
			</view>
			<view class="u-flex">
				<text class="text-area u-m-l-22 u-m-t-28 u-m-r-52">职位：</text>
				<input type="text" class="company u-m-t-26" />
			</view>
			<view class="u-flex u-m-t-40">
				<text class="text-area u-m-l-22 u-m-t-28 u-m-r-52">金额：</text>
				<input type="text" class="money u-m-t-26" />
			</view>
			<view class="des-area u-flex u-m-t-40">
				<text class="text-area u-m-l-22 u-m-t-28 u-m-r-52">描述：</text>
				<input type="text" class="describe u-m-t-26" />
			</view>
			<view class="u-m-t-40">
				<text class="text-area u-m-l-22 u-m-t-28 u-m-r-52">内推人</text>
			</view>
		</view>
		
		<view class="">
			<view class="pushman u-m-l-22 u-flex u-m-t-40" v-for="(item,index) in pushManList" :key="item.id" >
				<view class="u-flex-wrap ">
					<view class="nickname  u-m-l-36 u-m-b-38 ">{{item.nickname}}</view>
					<image class="head-img u-m-l-42" :src="imgUrl" mode="widthFix"></image>
					
				</view>
				<view class="u-flex-wrap">
					<view class="u-flex">
						<view class=" u-m-t-14 u-m-l-74">{{item.company}}</view>
						<view class="post u-m-t-14 u-m-l-68">{{item.post}}</view>
					</view>
					<view class="u-flex">
						<view class="success-rate  u-m-t-6">成功率：{{item.successRate}}</view>
						<button class="btn u-m-l-8 u-m-t-0" >选他</button>
					</view>
					<view class="u-flex">
						<view class="u-m-l-74 u-m-b-24">标语：{{item.title}}</view>
						<view class="u-m-l-38 u-m-t-0">好评率：{{item.rate}}</view>
						<button class="btn u-m-l-8 u-m-t-0" >移除</button>
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
				pushManList:[
					{
						id: 1,
						nickname:"TONY",
						imgUrl:"../../static/img/qq.png",
						company:"阿里巴巴",
						post:"前端工程师",
						successRate:"90%",
						title:"我是推神之神",
						rate:"96%"
						
					},
					{
						id: 2,
						nickname:"TONY",
						imgUrl:"../../static/img/qq.png",
						company:"阿里巴巴",
						post:"前端工程师",
						successRate:"90%",
						title:"我是推神之神",
						rate:"96%"
						
					},
					{
						id: 3,
						nickname:"TONY",
						imgUrl:"../../static/img/qq.png",
						company:"阿里巴巴",
						post:"前端工程师",
						successRate:"90%",
						title:"我是推神之神",
						rate:"96%"
						
					},
					{
						id: 4,
						nickname:"TONY",
						imgUrl:"../../static/img/qq.png",
						company:"阿里巴巴",
						post:"前端工程师",
						successRate:"90%",
						title:"我是推神之神",
						rate:"96%"
						
					},
					{
						id: 5,
						nickname:"TONY",
						imgUrl:"../../static/img/qq.png",
						company:"阿里巴巴",
						post:"前端工程师",
						successRate:"90%",
						title:"我是推神之神",
						rate:"96%"
						
					},
					{
						id: 6,
						nickname:"TONY",
						imgUrl:"../../static/img/qq.png",
						company:"阿里巴巴",
						post:"前端工程师",
						successRate:"90%",
						title:"我是推神之神",
						rate:"96%"
						
					}
				]
			}
		},
		methods: {
			
		}
	}
</script>

<style>
page{
	margin: 0;
	padding: 0;
	font-size: 26rpx;
}
.box{
	width: 100%;
	/* position: fixed; */
	background-color: #FFFFFF;
}
.center{
	width: 750rpx;
}
.text-area{
	/* flex-grow: 1; */
}
.company{
	width: 234rpx;
	height: 40rpx;
	/* flex-grow: 1; */
	background-color: #C4C4C4;
}
.money{
	width: 116rpx;
	height: 36rpx;
	background-color: #C4C4C4;
}
.describe{
	width: 536rpx;
	height: 256rpx;
	background-color: #C4C4C4;
}
.des-area{
	/* align-content: flex-start;	 */
	align-self: auto;
}
.pushman{
	width: 706rpx;
	height: 168rpx;
	background-color: #C4C4C4;
}
.nickname{
	/* margin-top: -10rpx; */
}
.btn{
	width: 72rpx;
	height: 52rpx;
	background: #00A8FF;
	border-radius: 12rpx;
	font-size: 26rpx;
	letter-spacing: 0.04em;
	line-height: 26rpx;	
}
.success-rate{
	margin-left: 346rpx;
}
.head-img {
	width: 60rpx;
	height: 60rpx;
	border-radius: 50%;
	z-index: 1;
}
</style>
