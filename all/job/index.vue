<template>
	<view>
		<view class="title">
			<scroll-view class="body" scroll-x="true" scroll-with-animation="true">
				<view @click="tab(1)">
					<text :class="tabindex==1?'c':''">找工作</text>
				</view>
				<view @click="tab(2)">
					<text :class="tabindex==2?'c':''">投递记录</text>
				</view>
				<view @click="tab(3)">
					<text :class="tabindex==3?'c':''">个人简历</text>
				</view>
			</scroll-view>
		</view>
		
		
		



		<view class=""  v-if="tabindex==1">
			<uni-search-bar v-model="ser" placeholder="搜索" cancelText="搜索" @cancel="search"></uni-search-bar>
		</view>
		<view class="sw"  v-if="tabindex==1">
			<swiper class="sw" :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000">
				<swiper-item v-for="img of img">
					<image class="img" :src="ip+img.advImg" mode="" style="width: 100%;" widthFix></image>
				</swiper-item>
			</swiper>
		</view>


		<view class="zw"  v-if="tabindex==1">
			<uni-grid :column="3" borderColor="#03a9f4" :square="false" :highlight="true">
				<uni-grid-item v-for="(zw, index) in zw"  :index="index" :key="index">
					<view class="grid-item-box" style="background-color: #fff;" @click="czw(zw.id)">
						<text class="text">{{zw.professionName}}</text>
					</view>
				</uni-grid-item>
			</uni-grid>
		</view>




		<view class="" style="padding-top: 50px;"  v-if="tabindex==1">
			<uni-list v-for="list of list">
				<uni-list-item clickable="true" @click="go(list.id)" :title="list.professionName"
					:note="list.obligation" :rightText="`${list.address==null?'':list.address}\n￥${list.salary}`">
				</uni-list-item>
			</uni-list>
		</view>
		
		<view v-if="list==''&&tabindex==1" class="liston">
			<view style="width: 20%; margin: 0 auto;">暂无数据</view>
		</view>
		
		
		
		<view class="" v-if="tabindex==2">
			<uni-list v-for="toudi of toudi">
				<uni-list-item :title="toudi.companyName" :note="toudi.postName" :rightText="`${toudi.satrTime}\n￥${toudi.money}`"></uni-list-item>
			</uni-list>
		</view>
		
		
		
		


		<view class="" v-if="tabindex==3">
			<!-- <uni-list>
				<uni-list-item title="查询简历" @click="find" clickable="true" showArrow="true"></uni-list-item>
				<uni-list-item title="新增简历" @click="add" clickable="true" showArrow="true"></uni-list-item>
				<uni-list-item title="修改简历" @click="update" clickable="true" showArrow="true"></uni-list-item>
			</uni-list> -->
			<view class="title1">
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
			<view class="title1">
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
				<button @click="go1" type="primary">确认修改</button>
			</view>
			
			
			
			<uni-popup ref="popup2" type="dialog">
				<uni-popup-dialog :type="msgType" cancelText="关闭" confirmText="同意" title="" 
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



		<uni-popup ref="alertDialog" type="dialog"  v-if="tabindex==1">
			<uni-popup-dialog type="error" cancelText="关闭" confirmText="同意" title="提示" :content="content"
				@confirm="dialogConfirm" @close="dialogClose"></uni-popup-dialog>
		</uni-popup>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				ip: '',
				ser: '',
				img: [],
				zw: '',
				code: 0,
				token: '',
				content: '',
				list: [],
				onlist:[],
				tit:0,
				tabindex:1,
				jianli:{},
				user:{},
				ta:null,
				styles:{
					color: '#2979FF',
					borderColor: '#2979FF'
				},
				jicun:'',
				panduan:0,
				msg:'',
				toudi:{}

			}
		},
		onLoad:function(o){
			if(o.id=='3'){
				this.tabindex=3
			}
		},
		onShow() {
			
			this.token = uni.getStorageSync('token')
			this.ip = uni.getStorageSync('ip')
			// uni.request({
			// 	url:`${this.ip}/prod-api/api/job/company/list`,
			// 	header:{'Content-Type':'application/x-www-form-urlencoded'},
			// 	success: (res) => {
			// 		console.log(res);
			// 	}

			// }),
			uni.request({
					url: `${this.ip}/prod-api/api/job/profession/list`,
					header: {
						'Content-Type': 'application/x-www-form-urlencoded',
						Authorization: uni.getStorageSync('token')
					},
					success: (res) => {
						console.log(res);
						this.zw = res.data.rows
						let arr={}
						// 数据去重
						this.zw=this.zw.reduce((cur,next)=>{
							arr[next.professionName]?"":arr[next.professionName]=true&&cur.push(next);
							return cur
						},[])
						
						
						
						// for(let i=0;i<this.zw.length;i++){
						// 	if(arr.indexOf(this.zw[i].professionName)===-1){
						// 		arr.push(this.zw[i])
						// 	}
						// }
						

					}

				}),
				uni.request({
					url: `${this.ip}/prod-api/api/rotation/list`,
					header: {
						'Content-Type': 'application/x-www-form-urlencoded'
					},
					success: (res) => {
						this.img = res.data.rows
						this.code = res.data.code
						console.log(this.img);
					}
				}),

				uni.request({
					url: `${this.ip}/prod-api/api/job/post/list`,
					header: {
						'Content-Type': 'application/x-www-form-urlencoded'
					},
					success: (res) => {
						console.log(res);
						this.list = res.data.rows.filter(s => s.professionName != null)
						this.onlist=this.list
					}
				}),
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
				}),
				uni.request({
					url:`${this.ip}/prod-api/api/job/deliver/list`,
					header:{
						'Content-Type':'application/x-www-form-urlencoded',
						Authorization:uni.getStorageSync('token')
					},
					success: (res) => {
						console.log(res);
						this.toudi=res.data.rows
					}
				})
				

		},
		methods: {
			search() {
				this.list=this.onlist
				let arr = []
				if (this.code != 200 || this.token == '' || this.token == undefined || this.token == null) {
					this.content = '请先登录'
					this.$refs.alertDialog.open()

				} else {
					this.list.forEach(s => {
						if (s.professionName.indexOf(this.ser) != -1) {
							arr.push(s)
							this.list = arr
						} 
						// else {
						// 	this.$refs.alertDialog.open()
						// 	this.content = '搜索内容不存在'
						// }
					})
					if(arr.length<1){
						this.list=[]
					}
				}



				console.log(this.code != 200, this.ip == '', this.ip == undefined, this.ip == null);
				console.log(this.code);
				console.log(this.ser);
			},
			dialogConfirm() {
				uni.navigateTo({
					url: "/pages/index/index"
				})
			},
			go(id) {
				uni.navigateTo({
					url: `/pages/all/job/list/list?id=${id}`
				})
			},
			czw(id){
				this.list=this.onlist
				this.list=this.list.filter(s=>s.professionId==id)
			},
			tab(index){
				this.tabindex=index
			},
			// find(){
			// 	uni.navigateTo({
			// 		url:'/pages/all/job/find/find'
			// 	})
			// },
			// add(){
			// 	uni.navigateTo({
			// 		url:'/pages/all/job/find/add/add'
			// 	})
			// },
			// update(){
			// 	uni.navigateTo({
			// 		url:"/pages/all/job/update/update"
			// 	})
			// },
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
			go1(){
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
	.img {
		border-radius: 5%;
	}

	.sw {
		width: 90%;
		margin: 0 auto;
		border-radius: 100%;
		height: 240px;
	}

	.title {
		display: flex;
		position: -webkit-sticky;
		position: sticky;
		top: var(--window-top);
		z-index: 99;
		flex-direction: row;
		margin: 0px;
		padding: 6px 0;
		background-color: #fff;
		border-bottom-style: solid;
		border-bottom-color: #E2E2E2;
	}

	.title::-webkit-scrollbar {
		display: none;
	}

	.body {

		white-space: nowrap;
	}

	.body view {
		display: inline-block;
		padding: 0 60upx;
		margin: 2% 0;
		border-right: 1upx solid #E2E2E2;
	}

	.body {
		text-align: center;
	}

	.grid-item-box {
		flex: 1;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		height: 36px;
	}
	.zw{
		padding-top: 50px;
		width: 80%;
		margin: 0 auto;
	}
	.text{
		text-align: center;
		font-size: 10px;
	}
	.liston{
		width: 100%;
	}
	.c{
		color: red;
	}
	.title1{
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
