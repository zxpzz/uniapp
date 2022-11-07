<template>
	<view>
		<view class="form">
			<form action="">
				<view class="">
					<view class="">
						<uni-easyinput placeholder="请输入反馈内容" maxlength="150"  type="textarea" autoHeight v-model="input" inputBorder></uni-easyinput>
					</view>
				</view>

			</form>
			<text>{{this.alt}}</text>
		</view>
		
		<view class="btu">
			<button @click="feed()">意见反馈</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				input:'',
				titleinput:'',
				alt:''
			}
		},
		methods: {
			feed(){
				uni.request({
					url:`${uni.getStorageSync('ip')}/prod-api/api/common/feedback`,
					method:'POST',
					header:{'Content-Type':'application/json',Authorization:uni.getStorageSync('token')},
					data:{
						"content": this.input,
						"title": "发现错误"	
					},
					success: (res) => {
						this.input=''
						this.alt=res.data.msg
					}
				})
			},
		}
	}
</script>

<style scoped>
	input{
		border: 1px solid black;
	}
	.form{
		width: 80%;
		margin: 0 auto;
	}
	button{
		background-color: aqua;
		color: red;
	}
	.btu{
		padding-top: 150px;
		width: 60%;
		margin: 0 auto;
	}
</style>
