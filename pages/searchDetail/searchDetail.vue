<template>
	<view class="content">
		<view class="search-box">
			<u-search shape="round" :clearabled="false" :animation="false" :show-action="false" height="64" placeholder="请输入工作或公司"
			 v-model="searchWord" bg-color='#F2F2F2' @search="search" ></u-search>
		</view>
	</view>
</template>

<script>
	export default {
		onLoad(option) { //option为object类型，会序列化上个页面传递的参数
		        console.log(option.name); //打印出上个页面传递的参数。
				this.searchWord=option.name
		    },
		data() {
			return {
				searchWord:'',
				lastSearch:[],
			}
		},
		methods: {
			search(){
				uni.navigateTo({
					url:'../search/search?name='+this.searchWord
				})
				this.lastSearch=this.lastSearch.concat(this.searchWord)
				uni.setStorage({
				    key: 'storage_key',
				    data: this.lastSearch,
				    success: function () {
				        console.log('success');
				    }
				});
			}
		}
	}
</script>

<style>
	.page{
		width: 100%;
	}
.search-box{
	width: 100%;
}
.content{
	width: 100%;
	padding: 0 30rpx;
}
</style>
