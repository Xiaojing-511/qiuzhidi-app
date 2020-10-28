 <template>
	<view class="content">
		<u-line border-style="solid" color="#EEEEEE"></u-line>
		<view class="content-top">
			<view class="company-name u-flex">
				<view class="company-name-left">
					企业名称:
				</view>
				<u-input class="company-name-input" v-model="company" placeholder="可多选" placeholder-style="color:#999999; font-size:24rpx; font-family:PingFangSC-Regular;font-weight:Regular"></u-input>
				<u-tag class="company-tag " closeable @close="clickCompanyTag(item,index)" v-for="(item,index) in companies" :text="companies[index]"></u-tag>
				<view class="company-name-right" @click="addCompany">
					添加
				</view>
			</view>
			<u-line border-style="solid" color="#EEEEEE"></u-line>
			<view class="position u-flex">
				<view class="position-left">
					职&emsp;&emsp;位:
				</view>
				<u-input class="position-input" v-model="post" placeholder="可多选" placeholder-style="color:#999999; font-size:24rpx; font-family:PingFangSC-Regular;font-weight:Regular"></u-input>
				<u-tag class="postion-tag" closeable @close="clickPostionTag(item,index)" v-for="(item,index) in position" :text="position[index]"></u-tag>
				<view class="position-right" @click="addPositon">
					添加
				</view>
			</view>
			<u-line border-style="solid" color="#EEEEEE"></u-line>
			<view class="salary u-flex">
				<view class="salary-left">
					金&emsp;&emsp;额:
				</view>
				<u-input class="salary-input" v-model="money" :placeholder="placeholder"></u-input>
			</view>
		</view>
		<view class="content-solid"></view>
		<view class="content-describe u-flex">
			<view class="describe-left">
				描&emsp;&emsp;述：
			</view>
			<u-input class="describe-input" v-model="describes" placeholder="选填" placeholder-style="color:#999999 font-size:24rpx; font-family:PingFangSC-Regular; font-weight:Regular;margin-left:29rpx"></u-input>
		</view>
		<u-button @click="issue" :custom-style="customStyle">发布</u-button>
		<view class="solid-bottom"></view>
		<u-toast ref="uToast"></u-toast>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				company:'',
				companies:[],
				post:'',
				position:[],
				money: '',
				describes: '',
				placeholder:'',
				customStyle:{
					width:"559rpx",
					height:"90rpx",
					// marginLeft:"76rpx",
					// marginRight:"75rpx",
					// marginTop:"400rpx",
					// margin:"0 75rpx 37rpx 76rpx",
					backgroundColor:"#00A8FF",
					borderRadius:"5rpx",
					// padding:"31rpx 271rpx 30rpx 270rpx"
					color:"#FFFFFF",
					fontSize:"30rpx",
					fontFamily:"PingFangSC-Regular",
					fontWeight:"Regular",
					margin:"0 auto",
					
				}
			}
		},
		methods:{
			addCompany(){
				if (this.companies.length < 3) {
					this.companies.push(this.company);
					this.company = ""
					console.log(this.companies)
				} else {
					this.company=""
					console.log("不能超过三个")
					this.showToast('添加企业不能超过三个','error')
				}
			},
			addPositon(){
				if(this.position.length<3){
					this.position.push(this.post);
					this.post=""
					console.log(this.position)
				}else{
					this.post=""
					console.log("不能超过三个")
					this.showToast('添加职位不能超过三个','error')
				}
			},
			issue(){
				if (this.companies == '' || this.position == '' || this.money == '') {
					console.log("输入信息不完整")
					this.showToast('输入信息不完整','error')
				} else {
					this.$u
						.post('/ipl/push', {
							companies: this.companies,
							position: this.position,
							money: this.money,
							describes: this.describes
						})
						.then(res=>{
							console.log(res)
						})
						.catch(res=>{
							console.log("error")
							this.showToast('请重试','error')
						})
						// console.log(this.companies,this.position,this.money,this.describes)
						// this.companies=[],
						// this.position=[],
						// this.money='',
						// this.describes=''
				}
			},
			showToast(title,type){
				this.$refs.uToast.show({
					title:title,
					type:type,
				})
			},
			clickCompanyTag(item,index){
				this.companies.splice(index,1)
				console.log(this.companies)
				
			},
			clickPostionTag(item,index){
				this.position.splice(index,1)
				console.log(this.position)
			}
			
		}
	}
</script>

<style>
	page{
		padding: 0;
		margin: 0;
	}
	.content{
		width: 750rpx;
		height: 1447rpx;
		/* background-color: #007AFF; */
	}
	.content-top{
		height: 271rpx;
		width: 690rpx;
		margin: 0 auto;
		/* background-color: #007AFF; */
	}
	.company-name{
		height: 89rpx;
		width: 100%;
		/* background-color: #007AFF; */
	}
	.company-name-left{
		margin-left: 21rpx;
		margin-right: 32rpx;
		font-size: 24rpx;
		color: #333333;
		font-family: PingFangSC-Regular;
		font-weight: Regular;
		width: 104rpx;
	}
	.company-name-input,.position-input{
		width: 120rpx;
	}
	.company-name-right,.position-right{
		color: #00A8FF;
		font-size: 24rpx;
		/* align-self: flex-end; */
		margin-left: auto;
		margin-right: 30rpx;
		width: 50rpx;
	}
	.position{
		width: 100%;
		height: 88rpx;
		/* background-color: #007AFF; */
	}
	.position-left{
		margin-left: 22rpx;
		font-size: 24rpx;
		color: #333333;
		margin-right: 32rpx;
	}
	.salary{
		width: 100%;
		height: 90rpx;
		/* background-color: #007AFF; */
	}
	.salary-left{
		margin-left: 21rpx;
		margin-right: 32rpx;
		font-size: 24rpx;
		font-family: PingFangSC-Regular;
		font-weight: Regular;
		color: #333333;
	}
	.content-solid{
		width: 100%;
		height: 20rpx;
		background-color: #EEEEEE;
	}
	.content-describe{
		margin-left: 51rpx;
		margin-top: 34rpx;
		margin-right: 30rpx;
	}
	.describe-left{
		color: #333333;
		font-size: 24rpx;
		font-family: PingFangSC-Regular;
		font-weight: Regular;
		align-self: flex-start;
		/* margin-right: 48rpx; */
	}
	.describe-input{
		background-color: #EEEEEE;
		width: 525rpx;
		height: 281rpx;
		margin-left: 24rpx;
		border-radius: 20rpx;
		margin-bottom: 682rpx;
	}
	.solid-bottom{
		background-color: #000000;
		width: 229.6rpx;
		height: 7.6rpx;
		margin: 0 auto;
		margin-top: 37rpx;
		opacity: 0.5;
	}
	.company-tag,.postion-tag{
		padding: 10rpx;
		font-size: 20rpx;
		width: 100rpx;
		height: 100%;
		margin-top: 20rpx;
	}
</style>
