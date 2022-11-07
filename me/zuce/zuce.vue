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
					<uni-icons type="person-filled"></uni-icons>
				</view>
				<view class="input">
					<uni-easyinput v-model="uname" type="text" :styles="styles" placeholder="请输入昵称"></uni-easyinput>
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
			<view class="body1">
				<view class="icon">
					<uni-icons type="phone-filled"></uni-icons>
				</view>
				<view class="input">
					<uni-easyinput v-model="phone" type="number" placeholder="请输入手机号"></uni-easyinput>
				</view>
			</view>
			
			<view class="body2">
				<view class="" @click="denglu">
					<p style="font-size: 12px;">已有帐号，立即登录</p>
				</view>
				
			</view>
			
		</view>
		
		<view class="but">
			<button type="primary"  @click="login">注册</button>
		</view>
		
		
		
		<uni-popup ref="alertDialog" type="dialog">
			<uni-popup-dialog type="error" cancelText="关闭" confirmText="同意" title="提示" :content="title"
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
				ip:'',
				phone:'',
				uname:'',
				title:'',
				code:0
				
			}
		},
		onShow() {
			this.ip=uni.getStorageSync('ip')
		},
		methods: {
			denglu(){
				uni.navigateTo({
					url:'../denglu/denglu'
				})
			},
			login(){
				uni.request({
					url:`${this.ip}/prod-api/api/register`,
					method:'POST',
					header:{'Content-Type':'application/json'},
					data:{
						'userName':this.name,
						"nickName": this.uname,
						"password": this.pass,
						"phonenumber": this.phone,
					},
					success: (res) => {
						this.title=res.data.msg
						this.$refs.alertDialog.open()
						this.code=res.data.code
						console.log('ssssssssssssssssssssssss');
						console.log(res)
					}
				})
			},
			dialogConfirm(){
				if(this.code == 200){
					uni.navigateTo({
						url:'/pages/me/denglu/denglu'
					})
				}
				
			}
		}
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
