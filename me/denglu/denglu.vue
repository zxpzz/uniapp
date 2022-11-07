<template>
	<view>
		<view class="title"></view>
		<view class="body">
			<view class="body1">
				<view class="icon">
					<uni-icons type="person-filled"></uni-icons>
				</view>
				<view class="input">
					<uni-easyinput v-model="name" type="text" :styles="styles" placeholder="请输入账号"></uni-easyinput>
				</view>
			</view>


			<view class="body1">
				<view class="icon">
					<uni-icons type="locked-filled"></uni-icons>
				</view>
				<view class="input">
					<uni-easyinput v-model="pass" type="password" placeholder="请输入密码"></uni-easyinput>
				</view>
			</view>
			
			
			
			<view class="body2">
				<view class="" @click="zuce">
					<p style="font-size: 12px;">暂无账号,立即注册</p>
				</view>
				
			</view>
			
			
		</view>
		
		
		
		<view class="but">
			<button type="primary"  @click="login">登录</button>
		</view>
		
		
		
		<uni-popup ref="alertDialog" type="dialog">
			<uni-popup-dialog type="error" cancelText="关闭" confirmText="同意" title="提示" content="账号或密码错误"
				@confirm="dialogConfirm" @close="dialogClose"></uni-popup-dialog>
		</uni-popup>




	</view>
</template>

<script>
	export default {
		data() {
			return {
				styles: {
					color: '#2979FF',
					borderColor: '#2979FF'
				},
				name:'',
				pass:'',
				ip:''
			}
		},
		onShow() {
			this.ip=uni.getStorageSync('ip')
		},
		methods: {
			login(){
				uni.request({
					url: `${this.ip}/prod-api/api/login`,
					method: 'POST',
					header: {
						'Content-Type': 'application/json'
					},
					data: JSON.stringify({
						"username": this.name,
						"password": this.pass
					}),
					success: (res) => {
						console.log(res)
						console.log(res.data.code==200);
						if(res.data.code==200){
							uni.setStorageSync('token', res.data.token)
							uni.switchTab({
								url:'/pages/me/me'
							})
							// uni.navigateTo({
							// 	url:'/pages/me/me'
							// })
						}else{
							this.$refs.alertDialog.open()
						}
						
				
					}
				})
			},
			zuce(){
				uni.navigateTo({
					url:'../zuce/zuce'
				})
			}

		},
	}
</script>

<style scoped>
	.body {
		display: flex;
		padding-top: 30px;
		flex-direction: column;
		width: 90%;
		margin: 0 auto;
		background-color: #f7f7f7;
		border-radius: 5%;
	}
	.body1{
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		width: 100%;
		padding-bottom: 10px;
		padding-top: 10px;
		border-bottom: 1px grey solid;
	}
	.icon{
		/* flex-grow: 1; */
		width: 10%;
		text-align: center;
	}
	.input{
		/* flex-grow: 6; */
		width: 90%;
	}
	.body2{
			display: flex;
			flex-direction: row;
			align-items: center;
			justify-content: center;
			width: 100%;
			padding-bottom: 20px;
			padding-top: 20px;
			/* border-bottom: 1px grey solid; */
	}
	.but{
		width: 60%;
		margin: 0 auto;
		padding-top: 60px
	}
	.title{
		padding-top: 50px;
	}
</style>
