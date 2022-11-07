<template>
	<view>
		<uni-list>
			<uni-list-item title="起点" :rightText="xiangxi.first"></uni-list-item>
			<uni-list-item title="终点" :rightText="xiangxi.end"></uni-list-item>
			<uni-list-item title="票价" :rightText="`${xiangxi.price}元`"></uni-list-item>
			<uni-list-item title="里程" :rightText="`${xiangxi.mileage}km`"></uni-list-item>
			<uni-list-item title="日期" :rightText="xiangxi.createTime"></uni-list-item>
			<!-- <uni-datetime-picker type="date" :clear-icon="false" v-model="single" @maskClick="maskClick" /> -->
		</uni-list>
		
		
		
		<view class="btn">
			<view class="">
				<button type="primary" @click="add">提交订单</button>
			</view>
			<view class="">
				<button type="warn" @click="fanhui">返回上一级</button>
			</view>
			
			
		</view>
		
		
		<uni-popup ref="pop">
			<uni-popup-dialog title="系统提示" confirmText="确定" cancelText="取消" :content="conten"></uni-popup-dialog>
		</uni-popup>
		
			
	</view>
	
	
</template>

<script>
	export default {
		data() {
			return {
				id:'',
				xiangxi:{},
				single:'',
				conten:''
			}
		},
		onLoad:function(o){
			this.id=o.id
		},
		onShow() {
			uni.request({
				url:`${uni.getStorageSync('ip')}/prod-api/api/bus/line/${this.id}`,
				header:{
					'Content-Type':'application/x-www-form-urlencoded',
					Authorization:uni.getStorageSync('token')
				},
				success: (res) => {
					console.log(res);
					this.xiangxi=res.data.data
				}
				
			})
		},
		methods: {
			add(){
				
				uni.request({
					url:`${uni.getStorageSync('ip')}/prod-api/api/bus/order`,
					method:'POST',
					header:{
						'Content-Type':'application/json',
						Authorization:uni.getStorageSync('token')
					},
					data:{
						"start":this.xiangxi.first,
						"end":this.xiangxi.end,
						"price":this.xiangxi.price,
						"path":this.xiangxi.name,
						"status":0
					},
					success: (res) => {
						console.log(res);
						// console.log(res.data.code);
						if(res.data.code==200){
							uni.navigateTo({
								url:`/pages/me/order/xiangxi/xiangxi?orderNum=${res.data.orderNum}`
							})
							// this.conten=res.data.msg
							// this.$refs.pop.open()
							
						}
						// console.log(res);
					}
					
				})
			},
			fanhui(){
				uni.navigateTo({
					url:'/pages/all/bus_query/custom_shuttle'
				})
			}
			
		}
	}
</script>

<style>
	.btn{
		padding-top: 100px;
		width: 50%;
		margin: 0 auto;
	}
	.btn view{
		padding-top: 10px;
	}

</style>
