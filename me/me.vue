<template>
	<view>
		<view class="me">
			<view class="header">
				<image :class="img1"  v-if="userimg || !userip" src="../../static/imgs/home_icon4_c.png" mode="aspectFit"></image>
				<image :class="img2"  v-if="usersimg" :src="ip+users.avatar" mode="aspectFit"></image>
				<text class="nickname" v-if="!userip&&users.nickName!='undefined'">{{users.nickName}}</text>
				<view class="nickname" v-if="userip" @click="denglu">点击登录</view>
				<view class=""></view>
			</view>
			<view class="">
				<uni-list>
					<uni-list-item title="个人信息" showArrow="true" clickable="true" @click="infor" icon></uni-list-item>
					<uni-list-item title="我的订单" showArrow="true" @click="order" clickable="true" ></uni-list-item>
					<uni-list-item title="修改密码" showArrow="true" clickable="true" @click="password"></uni-list-item>
					<uni-list-item title="意见反馈" showArrow="true" clickable="true" @click="feed"></uni-list-item>
					<uni-list-item title="关于" showArrow="true"  clickable="true" ></uni-list-item>
					
				</uni-list>
			</view>

		</view>
		
		<view class="but">
			<button type="primary" @click="exit">退出</button>
		</view>
		




		<uni-popup ref="alertDialog" type="dialog">
			<uni-popup-dialog type="error" cancelText="关闭" confirmText="同意" title="提示" content="请登录后访问"
				@confirm="dialogConfirm" @close="dialogClose"></uni-popup-dialog>
		</uni-popup>


	</view>

</template>

<script>
	export default {
		data() {
			return {
				users: {},
				userimg: false,
				usersimg: false,
				ip: '',
				userip: true,
				img1:'i2',
				img2:'i1',
				styles: {
					'background': '#aaffff',
					'border-bottom': 'black 1px solid',
					'font-size':'20px'
				}
			}
		},

		onShow() {

			// console.log(typeof(uni.getStorageSync('token')));
			// console.log(uni.getStorageSync('token'));
			this.ip=''
			this.ip = uni.getStorageSync('token')
			console.log(this.ip == '' || this.ip == undefined || this.ip == null);
			uni.request({
				url: 'http://10.1.33.249:10001/prod-api/api/common/user/getInfo',
				header: {
					'Content-Type': 'application/x-www-form-urlencoded',
					Authorization: uni.getStorageSync('token')
				},
				success: (res) => {
					console.log(res);
					// uni.setStorageSync('nickName',res.data.user.nickName)

					if (this.ip == '' || this.ip == undefined || this.ip == null) {
						this.userip = true
						
					} else {
						this.userip = false
						this.users = res.data.user
						if (this.users.avatarz == '') {
							this.userimg = true
						} else {
							this.img1='i2'
							this.img2='i1'
							this.usersimg = true
						}
					}
				}
			})

		},
		methods: {
			password() {
				if (!this.userip) {
					uni.navigateTo({
						url: '/pages/me/password/password'
					})
				} else {
					this.$refs.alertDialog.open()
				}

			},
			feed() {
				if (!this.userip) {
					uni.navigateTo({
						url: '/pages/me/feed/feed'
					})
				} else {
					this.$refs.alertDialog.open()
				}

			},
			infor() {
				if (!this.userip) {
					uni.navigateTo({
						url: '/pages/me/infor/infor'
					})
				} else {
					this.$refs.alertDialog.open()
				}

			},
			denglu() {
				uni.navigateTo({
					url: '/pages/me/denglu/denglu'
				})

			},
			exit(){
				uni.removeStorageSync('token')
				uni.navigateTo({
					url:'/pages/me/denglu/denglu'
				})
			},
			order(){
					if (!this.userip) {
						uni.navigateTo({
							url:'/pages/me/order/order'
						})
					} else {
						this.$refs.alertDialog.open()
					}	
			}


		}
	}
</script>

<style scoped>
	.header {
		padding-top: 30px;
		display: flex;
		justify-content: flex-start;
		align-items: center;
		flex-direction: column;
		background-color: #fff;
		height: 150px;
		border-bottom: black 1px solid;
	}

	.header image {
		/* width: 20%; */
		height: 76px;
		border-radius: 100%;

	}

	.header text {
		/* width: 10%; */
		text-align: center;
	}

	.nickname {
		padding-top: 20px;
		font-size: 25px;
		color: red;
	}

	
	.i1{
		display: none;
	}
	.i2{
		display: block;
	}
	.but{
		padding-top: 50px;
		width: 40%;
		margin: 0 auto;
	}
</style>
