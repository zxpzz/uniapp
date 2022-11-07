<template>
	<view>
		<view class="title">
			<view class="me">
				个人信息
			</view>
			<uni-list>
				<uni-list-item title="昵称" @click="info" clickable="true" :rightText="user.nickName"></uni-list-item>
				<uni-list-item title="邮件" @click="info" clickable="true"  :rightText="user.email!=''?user.email:'暂未添加'"></uni-list-item>
				<uni-list-item title="电话" @click="info" clickable="true"  :rightText="user.phonenumber"></uni-list-item>
				<uni-list-item title="性别" @click="info" clickable="true"  :rightText="user.sex==0?'男':'女'"></uni-list-item>
			</uni-list>
		</view>
		
		
		<view class="title">
			<view class="me">
				求职信息
			</view>
			<uni-list>
				<uni-list-item title="工作经验" @click="up(0)" clickable="true" :rightText="jianli.experience==null?'暂未填写':jianli.experience"></uni-list-item>
				<uni-list-item title="最高学历" @click="up(1)" clickable="true" :rightText="jianli.mostEducation==null?'暂未填写':jianli.mostEducation"></uni-list-item>
				<uni-list-item title="现居住地" @click="up(2)" clickable="true" :rightText="jianli.address==null?'暂未填写':jianli.address"></uni-list-item>
				<uni-list-item title="期望职位" @click="up(3)" clickable="true" :rightText="jianli.positionId==null?'暂未填写':jianli.positionId"></uni-list-item>
				<uni-list-item title="期望薪资" @click="up(4)" clickable="true" :rightText="jianli.money==null?'暂未填写':jianli.money"></uni-list-item>
				<uni-list-item title="教育经历" @click="up(5)" clickable="true" :rightText="jianli.education==null?'暂未填写':jianli.education"></uni-list-item>
				<uni-list-item title="个人简介" clickable="true" @click="up(6)">
					<template slot="footer">
						<view class="foot">{{jianli.individualResume}}</view>
					</template>
				</uni-list-item>
				
			</uni-list>
			
		</view>
		
		
		<view class="btn">
			<button @click="go" type="primary">确认修改</button>
		</view>
		
		
		
		<uni-popup ref="popup2" type="dialog">
			<uni-popup-dialog :type="msgType" cancelText="关闭" confirmText="同意" title="" content="欢迎使用 uni-popup!"
				@confirm="confirm2" @close="close2">
				<view class="uni-px-5 uni-pb-5">
					<uni-easyinput type="text" autoHeight :styles="styles" v-model="jicun"></uni-easyinput>
				</view>
			</uni-popup-dialog>
		</uni-popup>
		
		
		
		<uni-popup ref="alertDialog" type="dialog">
			<uni-popup-dialog type="error" cancelText="关闭" confirmText="同意" title="提示" :content="msg"
				@confirm="dialogConfirm" @close="dialogClose"></uni-popup-dialog>
		</uni-popup>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				jianli:{},
				user:{},
				ta:null,
				styles:{
					color: '#2979FF',
					borderColor: '#2979FF'
				},
				jicun:'',
				panduan:0,
				msg:''
			}
		},
		onShow() {
			uni.request({
				url: 'http://10.1.33.249:10001/prod-api/api/common/user/getInfo',
				header: {
					'Content-Type': 'application/x-www-form-urlencoded',
					Authorization: uni.getStorageSync('token')
				},
				success: (res) => {
					this.user=res.data.user
					console.log(this.user);
				}
			})
			
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
			up(e){
				this.panduan=e
				switch(this.panduan){
					case 0 :
						this.jicun=this.jianli.experience
					break;
					case 1:
						this.jicun=this.jianli.mostEducation
					break;
					case 2:
						this.jicun=this.jianli.address
					break;
					case 3:
						this.jicun=this.jianli.positionId
					break;
					case 4:
						this.jicun=this.jianli.money
					break;
					case 5:
						this.jicun=this.jianli.education
					break;
					case 6:
						this.jicun=this.jianli.individualResume
					break;	
					
					
				}
					
					this.$refs.popup2.open()

				
			},
			confirm2(){
				
				switch(this.panduan){
					case 0 :
						this.jianli.experience=this.jicun
					break;
					case 1:
						this.jianli.mostEducation=this.jicun
					break;
					case 2:
						this.jianli.address=this.jicun
					break;
					case 3:
						this.jianli.positionId=this.jicun
					break;
					case 4:
						this.jianli.money=this.jicun
					break;
					case 5:
						this.jianli.education=this.jicun
					break;
					case 6:
						this.jianli.individualResume=this.jicun
					break;	
					
					
				}
			this.jicun=''

			},
			go(){
				uni.request({
					url:`${uni.getStorageSync('ip')}/prod-api/api/job/resume`,
					method:'PUT',
					header:{'Content-Type':'application/json',Authorization:uni.getStorageSync('token')},
					data:JSON.stringify(this.jianli),
					success: (res) => {
						console.log(res);
						this.msg=res.data.msg
						this.$refs.alertDialog.open()
					}
					
					
				})
			},
			info(){
				uni.navigateTo({
					url:'/pages/me/infor/infor'
				})
			}
		}
	}
</script>

<style scoped>
	.title{
		padding-top: 20px;
		display: flex;
		flex-direction: column;
		width: 100%;
	}
	.me{
		width: 100%;
		/* margin: 0 auto; */
		text-align: center;
		font-size: 20px;
		padding-top: 10px;
		padding-bottom: 10px;
		border-bottom: 5px #E2E2E2 solid;
		border-top: 3px #E2E2E2 solid;
	}
	.foot{
		width: 100px;
		font-size: 10px;
		color: #949494;
	}
	.btn{
		width: 50%;
		margin: 0 auto;
		padding-top: 50px;
	}

</style>
