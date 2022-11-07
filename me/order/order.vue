<template>
	<view>
		<view class="title">
			<scroll-view class="body" scroll-x="true" scroll-with-animation="true">
				<view class="" @click="tab(2)">
					<text :class="sta==2?'styles':''">全部</text>
				</view>
				<view class="" @click="tab(1)">
					<text :class="sta==1?'styles':''">已支付</text>
				</view>
				<view class="" @click="tab(0)">
					<text :class="sta==0?'styles':''">未支付</text>
				</view>
			</scroll-view>
		</view>
		
		<view class="">
			<uni-list v-for="list of list">
				<uni-list-item clickable="true" @click="xiangqing(list.orderNum)" :title="`订单号：${list.orderNum}`" note="订单类型: 智慧巴士" :rightText="`生成时间：${list.createTime}`"></uni-list-item>
			</uni-list>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				sta:2,
				list:[],
				jicun:[]
				
			}
		},
		onShow() {
			uni.request({
				url:`${uni.getStorageSync('ip')}/prod-api/api/bus/order/list`,
				header:{'Content-Type':'application/x-www-form-urlencoded',Authorization:uni.getStorageSync('token')},
				success: (res) => {
					console.log(res);
					this.list=res.data.rows
					this.jicun=this.list
				}
				
			})
		},
		methods: {
			tab(e){	
				this.sta=e
				this.list=[]
				this.list=this.jicun
				switch(e){
					case 0:
						this.list=this.list.filter(s=>s.status==e)
					break;
					case 1:
						this.list=this.list.filter(s=>s.status==e)
					break;
					case 2:
						this.list=this.jicun
					break;
				}
				

				// 	console.log(e);
				// 	console.log(this.jicun);
				// 	console.log(this.list[0]);

				// 	console.log(this.jicun[0]);
				// else
				// 	this.list=this.jicun
				
			},
			xiangqing(orderNum){
				console.log(orderNum);
				uni.navigateTo({
					url:`/pages/me/order/xiangxi/xiangxi?orderNum=${orderNum}`
				})
			}
		}
	}
</script>

<style scoped>
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
		text-align: center;
		white-space: nowrap;
	}
	.body view {
		display: inline-block;
		padding: 0 60upx;
		margin: 2% 0;
		border-right: 1upx solid #E2E2E2;
	}
	.styles{
		color: red;
	}


</style>
