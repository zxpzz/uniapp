<template>
	<view>
		<view class="body">
			<view class="">
				<view class="img">
					<image :src='users.avatar' mode=""></image>
				</view>
			</view>


			<view class="">
				<uni-list-item title="昵称" @click="open" clickable="true" :rightText="users.nickName"></uni-list-item>
			</view>

			<view class="">
				<uni-list-item title="性别" clickable="true" @click="open2" :rightText="users.sex">
				</uni-list-item>
			</view>

			<view class="">
				<uni-list-item clickable="true" @click="open1" title="手机号" :rightText="phonenum">
				</uni-list-item>
			</view>
			
			<view class="text">
				<p>{{this.title}}</p>
			</view>



			<uni-popup ref="popup" type="dialog">
				<uni-popup-dialog placeholder="请输入要修改的内容" mode="input" message="成功消息" :duration="2000"
					:before-close="true" @close="close" @confirm="confirm" v-model="users.nickName"></uni-popup-dialog>
			</uni-popup>

			<uni-popup ref="popup1" type="dialog">
				<uni-popup-dialog placeholder="请输入" mode="input"  type="number" message="成功消息" :duration="2000"
					:before-close="true" @close="close1" @confirm="confirm1" v-model="users.phonenumber">
				</uni-popup-dialog>
			</uni-popup>


			<uni-popup ref="popup2" type="dialog">
				<uni-popup-dialog :type="msgType" cancelText="关闭" confirmText="同意" title="" content="欢迎使用 uni-popup!"
					@confirm="confirm2" @close="close2">
					<view class="uni-px-5 uni-pb-5">
						<uni-data-checkbox v-model="radio1" :localdata="sex1"></uni-data-checkbox>
					</view>
				</uni-popup-dialog>



				
			</uni-popup>








		</view>
		<view class="but">
			<button @click="btn">修改信息</button>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				users: {},
				name: '',
				phone: '',
				showRight: false,
				radio1: 0,
				sex: '',
				radio1: 0,
				sex1: [{
					text: '男',
					value: 0
				}, {
					text: '女',
					value: 1
				}],
				title:'',
				phonenum:''

			}
		},
		onShow() {

			uni.request({
				url: `${uni.getStorageSync('ip')}/prod-api/api/common/user/getInfo`,
				header: {
					'Content-Type': 'application/x-www-form-urlencoded',
					Authorization: uni.getStorageSync('token')
				},
				success: (res) => {
					this.sex = res.data.user.sex
					console.log(res.data.user);
					if (res.data.user.avatar == '') {
						res.data.user.avatar = '../../../static/imgs/home_icon4_c.png'
					}

					switch (res.data.user.sex) {
						case '0':
							res.data.user.sex = '男';
							break;
						case '1':
							res.data.user.sex = '女';
							break;
						default:
							res.data.user.sex = '未知';
							break;
					}
					this.phone = res.data.user.phonenumber
					this.name = res.data.user.nickName


					this.users = res.data.user
					console.log(this.users);
					this.phonenum= this.users.phonenumber.replace(/^(\d{7})(\d+)/,"$1****")
					console.log(this.phonenum);
				}
			})
			
			

		},
		mounted() {
			
			
			
		},
		methods: {
			open() {
				this.$refs.popup.open()
			},
			close() {
				this.$refs.popup.close()
			},
			confirm(value) {
				this.users.nickName = value
				this.name = value
				this.$refs.popup.close()
			},
			open1() {
				this.$refs.popup1.open()
			},
			close1() {

				this.$refs.popup1.close()
			},
			confirm1(value) {
				this.phonenum = value.replace(/^(\d{7})(\d+)/,"$1****")
				this.phone = value
				this.$refs.popup1.close()
			},
			open2() {
				this.$refs.popup2.open()
			},
			close2() {
				this.$refs.popup2.close()
			},
			confirm2() {
				console.log(this.radio1);
				this.sex=this.radio1
				switch (this.sex) {
					case 0:
						this.users.sex = '男';
						break;
					case 1:
						this.users.sex = '女';
						break;
					default:
						this.users.sex = '未知';
						break;
				}
				
				
				this.$refs.popup.close()
			},
			// 打开窗口
			showDrawer(e) {
				this.$refs[e].open()
			},
			// 关闭窗口
			closeDrawer(e) {
				this.$refs[e].close()
			},
			btn() {
				uni.request({
					url: `${uni.getStorageSync('ip')}/prod-api/api/common/user`,
					method: 'PUT',
					header: {
						'Content-Type': 'application/json',
						Authorization: uni.getStorageSync('token')
					},
					data: {
						"nickName": this.name,
						"phonenumber": this.phone,
						"sex": this.sex
					},
					success: (res) => {
						this.title=res.data.msg
						
						setTimeout(()=>{
							this.title=''
						},5000)

					}

				})
			}
			

		},
		


		
	}
</script>

<style scoped>
	.body {
		width: 100%;
		display: flex;
		align-items: center;
		flex-direction: column;
	}

	.body view {
		width: 100%;
	}

	image {
		border-radius: 100%;
		width: 100px;
		height: 100px;
	}

	.img {
		background-color: white;
		width: 100px;
		height: 100px;
	}

	.but {
		padding-top: 150px;

	}

	.but button {
		width: 60%;
		margin: 0 auto;
		background-color: aqua;
		color: red;
	}

	.cbut {
		padding-top: 150px;
	}

	.cbut button {
		width: 30%;
		margin: 0 auto;
		color: red;
	}
	.text{
		margin: 0 auto;
		padding-top: 50px;
	}
	.text p{
		text-align: center;
		color: red;
	}
</style>
