<template>
	<view>
		<view class="from">
			<form action="">
				<view class="">
					<view class="text">
						原密码:
					</view>
					<view class="">
						<uni-easyinput type="password" v-model="oldpassword" placeholder="原密码"></uni-easyinput>
					</view>
				</view>
				<view class="">
					<view class="text">
						新密码:
					</view>
					<view class="">
						<uni-easyinput type="password" v-model="password" placeholder="新密码"></uni-easyinput> 
					</view>
				</view>
			</form>
			<text>{{this.title}}</text>
		</view>
		
		<view class="but">
			<button @click="pw()">修改密码</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				password:'',
				users:'',
				oldpassword:'',
				title:''
			}
		},
		methods: {
			pw(){
				
				uni.request({
					url:`http://10.1.33.249:10001/prod-api/api/common/user/resetPwd`,
					method:'PUT',
					header:{'Content-Type':'application/json',Authorization:uni.getStorageSync('token')},
					data:{
						"newPassword": this.password,
						"oldPassword": this.oldpassword
					},
					success: (res) => {
						this.password='',
						this.oldpassword='',
						this.title=res.data.msg
					}
				})
			}
		}
	}
</script>

<style scoped>
	.from{
		width: 90%;
		margin: 0 auto;
	}
	input{
		height: 50px;
		
		border: 1px solid black;
	}
	.but{
		padding-top: 250px;
	}
	button{
		background-color: aqua;
		color: red;
		margin: 0 auto;
		width: 70%;
	}
	.text{
		font-size: 20px;
		color: red;
	}



</style>
