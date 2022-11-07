<template>
	<view>
		<view class="">
			<uni-list>
				<uni-list-item title="职位名称" :rightText="job.name"></uni-list-item>
				<uni-list-item title="岗位职责" :rightText="job.obligation"></uni-list-item>
				<uni-list-item title="工资待遇" :rightText="`￥${job.salary}`"></uni-list-item>
				<uni-list-item title="公司地点" :rightText="job.address"></uni-list-item>
				<uni-list-item title="联系人" :rightText="job.contacts"></uni-list-item>
				<uni-list-item title="职位描述" :rightText="job.obligation"></uni-list-item>
				<uni-list-item title="职位需求" :rightText="job.need"></uni-list-item>
			</uni-list>
		</view>
		
		
		<view class="to">
			<view class="jianjie">
				<view class="title">
					<text>{{job.companyName}}</text>
				</view>
				
				<view class="body">
					<text>{{job.professionName}}</text>
				</view>
				
				
				<view class="und" v-if="job.companyName==null||job.professionName">
					公司详细内容暂无
				</view>
				
				
			</view>
			
			
			<view class="btn">
				<button @click="bt" type="primary">投递简历</button>
			</view>
			
			
			
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				id:'',
				ip:'',
				job:{},
				jianli:{}
			}
		},
		onLoad:function(o){
			this.id=o.id
			console.log(o.id);
		},
		onShow() {
			this.ip=uni.getStorageSync('ip')
			uni.request({
				url:`${this.ip}/prod-api/api/job/post/${this.id}`,
				header:{'Content-Type':'application/x-www-form-urlencoded'},
				success: (res) => {
					console.log(res);
					this.job=res.data.data
				}
			}),
			uni.request({
				url:`${uni.getStorageSync('ip')}/prod-api/api/job/resume/queryResumeByUserId/1`,
				header:{'Content-Type':'application/x-www-form-urlencoded',Authorization:uni.getStorageSync('token')},
				success: (res) => {
					this.jianli=res.data.data
					console.log(res);
				}
			})
		},
		methods: {
			bt(){
				uni.navigateTo({
					url:'/pages/all/job/index?id=3'
				})
				// if(this.jianli.experience==null||this.jianli.education==null){

				// }
				// uni.request({
				// 	url:`${this.ip}/prod-api/api/job/deliver`,
				// 	method:'POST',
				// 	header:{
				// 		'Content-Type':'application/json',
				// 		Authorization:uni.getStorageSync('token')
				// 	},
				// 	data:{
				// 		"companyId": this.job.companyId,
				// 		"money": this.job.salary,
				// 		"postId": this.professionId,
				// 		"postName": this.job.name,
				// 		"satrTime": Date,
				// 	},
				// 	success: (res) => {
				// 		console.log(res);
				// 		console.log(this.job.companyId,this.job.salary,this.job.professionId);
				// 	}
				// })
			}
		}
	}
</script>

<style scoped>
	.jianjie{
		display: flex;
		flex-direction: column;
		width: 80%;
		background-color: aliceblue;
		border-radius: 10%;
		margin: 0 auto;
	}
	.title{
		width: 20%;
		margin: 0 auto;
	}
	.und{
		text-align: center;
		width: 50%;
		margin: 0 auto;
	}
	.to{
		padding-top: 50px;
	}
	.btn{
		margin: 0 auto;
		padding-top: 50px;
		width: 50%;
	}

</style>
