<template>

	<view>
		<view class="header">
			<view class="image" >
				<cover-image  :src='companyLogo'></cover-image >
			</view>
			<view class="company">
				<u-field v-model="companyName" label="公司">
				</u-field>
			</view>
			<view class="companyAddress">
				<u-field v-model="companyAddress" label="公司地址" type="textarea">
				</u-field>
			</view>
		</view>
		<view class="context">
			<view class="position">
				<u-field label="职位" v-model="recruit_name" placeholder="请输入职位">
				</u-field>
			</view>
			<view class="salary">
				<u-field v-model="recruit_salary" label="工资" placeholder="请输入薪资">
				</u-field>
			</view>
			<view class="address">
				<u-field v-model="recruit_workspace" label="工作地点" placeholder="请输入工作地点">
				</u-field>
			</view>
			<view class="jobRequirement">
				<u-field v-model="skill_required" label="工作要求" placeholder="请输入工作要求" type="textarea">
				</u-field>
			</view>
			<view class="companyProfile">
				<u-field v-model="companyIntro" label="企业介绍" placeholder="" type="textarea">
				</u-field>
			</view>
			<view class="additionalRemarks">
				<u-field v-model="job_description" label="补充说明" placeholder="请输入补充说明" type="textarea">
				</u-field>
			</view>
		</view>
		<view class="footer">
			<view class="recruitType"> 
				<u-select v-model="show" :list="list" @click="choose" @confirm="confirm"></u-select>
				<u-button @click="show = true" class="recruitType">招聘类型</u-button>
			</view>
			<view >
				<button @click="publish" class='issue'>发布</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				show: false,
				list: [{
						value: '0',
						label: '实习'
					},
					{
						value: '1',
						label: '校招'
					},
					{
						value: '2',
						label: '社招'
					}
				],
				recruit_salary: '',
				recruit_name: '',
				recruit_workspace: '',
				department: '',
				type: '',
				skill_required: '',
				job_description: '',
				companyId: '',
				company: '',
				releaseDate: '',
				companyAddress: '',
				companyLogo: '',
				companyIntro: '',
				chooseType: '',
				memberId: '',
				companyName: '',
				id: '',
				e: '',
				getType: '',
				imgUrl: 'http://www.qingmengtech.com:8085/'
			}
		},
		methods: {
			onLoad() {
				this.$u.post('recruit/getCompanyInfo', {
					memberId: 70
				}).then(res => {
					this.recruit_workspace = res.recruit_workspace,
						this.companyName = res.companyName,
						this.companyIntro = res.companyIntro,
						this.companyLogo = this.imgUrl + res.companyLogo,
						this.companyAddress = res.companyAddress
				}).catch(res => {
					console.log('不对')
				})
			},
			confirm(e) {
				this.getType = e[0].value;
			},

			publish() {
				this.$u.post('recruit/releaseRecruit', {
					recruit_salary: this.recruit_salary,
					recruit_name: this.recruit_name,
					recruit_workspace: this.recruit_workspace,
					department: this.department,
					skill_required: this.skill_required,
					job_description: this.job_description,
					companyId: 70,
					type: this.getType
				}).then(res => {
					console.log('对')
				}).catch(res => {
					console.log('error')
				})
			},


		}
	}
</script>

<style lang="scss" scoped>
	page {
			margin: 0;
			padding: 0;
			position: relative;
		}
		
	.header{
			margin-left: $font_important_small;
			margin-top: $font_important_small;
			font-weight: 600;
			font-size: 36rpx;
			placeholder-style:$color_outline;
			.image {
				width: 100rpx;
				height: 100rpx;
				margin-left: 160rpx;
			}
			.company{
				
			}
			.companyAddress{
				 height: 80rpx;
			}
			
	}
	.context{
			margin-left: $font_important_small;
			font-weight: 600;
			font-size: 36rpx;
			placeholder-style:$color_outline;
			.jobRequirement{
				height: 80rpx;
			}
			.companyProfile{
				height: 80rpx;
			}
	}
	.footer{
			margin: auto;
			position: absolute;
			left: 0;
			right: 0;	
			
			.recruitType{
				width: 599rpx;
				height: 90rpx;
				margin: auto;
				position: absolute;
				left: 0;
				right: 0;
			}
			.issue{
				width: 599rpx;
				height: 90rpx;
				background: #00A8FF;
				border-radius: 5rpx;
				margin-top: 100rpx;
				
			}
			
	}
		
	
	
</style>
