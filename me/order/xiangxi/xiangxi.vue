<template>
	<view style="padding-top: 20px;">

		<uni-card padding="0" spacing="0" style="width: 80%; margin: 0 auto;" v-for="list of list">
			<template>
				<view class="title">
					<view class="tit">
						<p>订单：{{list.orderNum}}</p>
					</view>
					<view class="tit">
						<p>￥{{list.price}}</p>
					</view>
				</view>
			</template>
			<template>
				<view class="bus">
					<view class="buss">
						<p>{{list.start}}</p>
					</view>
					<view class="buss">
						<view class="">
							<image src="../../../../static/componentHL.png" mode=""></image>
						</view>
						
					</view>
					<view class="buss">
						<p>{{list.end}}</p>
					</view>
				</view>
			</template>
			<uni-list>
				<uni-list-item>
					<template v-slot:header>
						<p>线路</p>
					</template>
					<template v-slot:footer>
						<p>{{list.path}}</p>
					</template>
				</uni-list-item>
				<uni-list-item>
					<template v-slot:header>
						<p>乘车人</p>
					</template>
					<template v-slot:footer>
						<p>{{list.userName}}</p>
					</template>
				</uni-list-item>
				<uni-list-item>
					<template v-slot:header>
						<p>手机号</p>
					</template>
					<template v-slot:footer>
						<p>{{list.userTel}}</p>
					</template>
				</uni-list-item>
				<uni-list-item>
					<template v-slot:header>
						<p>预定时间</p>
					</template>
					<template v-slot:footer>
						<p>{{list.createTime}}</p>
					</template>
				</uni-list-item>
			</uni-list>
		</uni-card>
		<text>{{this.cs}}</text>


		<view class="btn" >
			<button v-if="list[0].status==0" type="primary" @click="zhifu">去支付</button>
		</view>
		<view class="but">
			<button type="warn" @click="fanhui">返回主页</button>
		</view>
		
		
		
		<uni-popup ref="pop" type="message">
			<uni-popup-message type="success" :duration="1000" message="支付成功" ></uni-popup-message>
		</uni-popup>
		
		
		
		<uni-popup ref="pop1" type="message">
			<uni-popup-message type="warning" :duration="1000" :message="msg" ></uni-popup-message>
		</uni-popup>
		
		
		
	</view>
	
	
	
	
	
	
</template>

<script>
	export default {
		data() {
			return {
				orderNum: '',
				list: {},
				msg:'',
				cs:''
			}
		},
		onLoad: function(o) {
			this.orderNum = o.orderNum
		},
		onShow() {
			uni.request({
				url: `${uni.getStorageSync('ip')}/prod-api/api/bus/order/list`,
				header: {
					'Content-Type': 'application/x-www-form-urlencoded',
					Authorization: uni.getStorageSync('token')
				},
				success: (res) => {
					this.list = res.data.rows.filter(s => s.orderNum == this.orderNum)
					console.log(this.list);
					
				}

			})
		},
		methods: {
			zhifu(){
				uni.request({
					url:`${uni.getStorageSync('ip')}/prod-api/api/bus/pay`,
					method:'POST',
					header:{
						'Content-Type':'application/json',
						Authorization:uni.getStorageSync('token')
					},
					data:{
						"orderNum": this.list[0].orderNum,
						"paymentType": "电子支付"
					},
					success: (res) => {
						console.log(res);
						if(res.data.code==200){
							this.$refs.pop.open()
						}else{
							this.msg=res.data.msg
							this.$refs.pop1.open()
						}
					}
				})
				
			},
			fanhui(){
				uni.switchTab({
					url:'/pages/index/index'
				})
			}
		}
	}
</script>

<style scoped>
	/* .body {
		display: flex;
		flex-direction: column;
		width: 80%;
		margin: 0 auto;
		background-color: cornsilk;
		border-radius: 5%;
	} */
	.title{
		padding-top: 15px;
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
		padding-bottom: 20px;
		border-bottom: 2px blue dashed;
	}
	.tit{
		flex-grow: 1;
		width: 50%;
	}
	.tit p{
		text-align: center;
		color: deepskyblue;
		font-size: 20px;
	}
	.tit image{
		width: 50px;
		height: 50px;
		border-radius: 20%;
	}
	.bus{
		padding-top: 20px;
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
		padding-bottom: 10px;
	}
	.buss{
		
		flex-grow: 1;
	}
	.buss p{
		text-align: center;
		font-size: 20px;
	}
	.buss view{
		width: 60px;
		height: 60px;
		margin: 0 auto;
	}
	.buss view image{
		width: 100%;
		height: 100%;

	}
	.btn{
		width: 50%;
		margin: 0 auto;
		padding-top: 100px;
	}
	.but{
		width: 50%;
		margin: 0 auto;
		padding-top: 20px;
	}
</style>
