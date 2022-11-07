<template>
	<view>
		<uni-list>
			<uni-list-item title="工作经验" :rightText="jianli.experience==null?'暂未填写':jianli.experience"></uni-list-item>
			<uni-list-item title="最高学历" :rightText="jianli.mostEducation==null?'暂未填写':jianli.mostEducation"></uni-list-item>
			<uni-list-item title="现居住地" :rightText="jianli.address==null?'暂未填写':jianli.address"></uni-list-item>
			<uni-list-item title="期望职位" :rightText="jianli.positionId==null?'暂未填写':jianli.positionId"></uni-list-item>
			<uni-list-item title="期望薪资" :rightText="jianli.money==null?'暂未填写':jianli.money"></uni-list-item>
			<uni-list-item title="教育经历" :rightText="jianli.education==null?'暂未填写':jianli.education"></uni-list-item>
			<uni-list-item title="个人简介" clickable="true" @click="up(6)">
				<template slot="footer">
					<view class="foot">{{jianli.individualResume}}</view>
				</template>
			</uni-list-item>
			
		</uni-list>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				ip:'',
				jianli:{}
			}
		},
		onShow() {
			this.ip=uni.getStorageSync('ip')
			console.log(this.token);
			uni.request({
				url:`${this.ip}/prod-api/api/job/resume/queryResumeByUserId/1`,
				header:{'Content-Type':'application/x-www-form-urlencoded',Authorization:uni.getStorageSync('token')},
				success: (res) => {
					this.jianli=res.data.data
					console.log(res);
				}
			})
		},
		methods: {
			
		}
	}
</script>

<style>
	.foot{
		width: 100px;
		font-size: 10px;
		color: #949494;
	}

</style>
