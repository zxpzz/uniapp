<template>
	<view>
		
		<uni-card v-for="bus of bus">
			<template>
				<view style="border-bottom: 1px dashed skyblue">
					<view class="title">
						<button type="primary" @click="go(bus.id)">{{bus.name}}</button>
					</view>
				</view>
				
			</template>
			<template>
				<view class="body">
					<view class="">
						<p>{{bus.first}}</p>
					</view>
					<view class="">
						<view class="img">
							<image src="../../../static/componentHL.png" mode=""></image>
						</view>
					</view>
					<view class="">
						<p>{{bus.end}}</p>
					</view>
				</view>
			</template>
			<template>
				<uni-list>
					<uni-list-item>
						<template v-slot:header>
							<p style="color:blue;">票价：￥{{bus.price}}</p>
						</template>
						<template v-slot:footer>
							<p style="color:red;">开始：{{bus.startTime}}</p>
						</template>
					</uni-list-item>
					<uni-list-item>
						<template v-slot:header>
							<p style="color:blue;">里程：{{bus.mileage}}km</p>
						</template>
						<template v-slot:footer>
							<p style="color: red;">结束：{{bus.endTime}}</p>
						</template>
					</uni-list-item>
				</uni-list>
			</template>
		</uni-card>
		
		
		
		
		
		
		
		
		
		<!-- <uni-list v-for="bus of bus">
			<uni-list-item @click="" clickable="true">
				<template v-slot:header >
					<view class="tit">
						<p>{{bus.name}}</p>
					</textarea></view>
				</template>
				<template v-slot:body>
					<view class="tit">
						<view class="tit1">
							<view class="p">
								<p>{{bus.first}}</p>
							</view>
							
							<view class="img">
								<image src="../../../static/componentHL.png" mode=""></image>
							</view>
							<view class="p">
								<p>{{bus.end}}</p>
							</view>
							    
						</view>
						<view class="tit1">
							<view class="">
								<p>{{bus.startTime}}</p>
							</view>
							<view class="img1">
								<image src="../../../static/imgs/right2.png" mode=""></image>
							</view>
							<view class="">
								<p>{{bus.endTime}}</p>
							</view>
								
							
						</view>
							
					</view>	
				</template>
				<template v-slot:footer>
					<view class="tit">
						<p>{{bus.mileage}}KM</p>
						<p>{{bus.price}}元</p>
					</view>
				</template>
			</uni-list-item>
		</uni-list> -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
				bus:[]
			}
		},
		onShow() {
			
			uni.request({
				url:`${uni.getStorageSync('ip')}/prod-api/api/bus/line/list`,
				header:{'Content-Type':'application/x-www-form-urlencoded'},
				success: (res) => {
					console.log(res);
					this.bus=res.data.rows
				}
			})
		},
		methods: {
			go(id){
				uni.navigateTo({
					url:`/pages/all/bus_query/index/queren/queren?id=${id}`
				})
			}
		}
	}
</script>

<style scoped>
	.title{
		width: 50%;
		margin: 0 auto;
		padding-top: 20px;
		border-bottom: 1px dashed skyblue;
		padding-bottom: 15px;
	}
	.body{
		padding-top: 20px;
		display: flex;
		flex-direction: row;
		justify-content: space-around;
		align-items: center;
	}
	.body view{
		flex-grow: 1;
	}
	.body view p{
		text-align: center;
		font-size: 15px;
	}
	.img{
		width: 60px;
		height: 60px;
		margin: 0 auto;
	}
	.img image{
		width: 100%;
		height: 100%;
		margin: 0 auto;
	}
	.body view p{
		font-size: 20px;
	}
	/* .tit{
		
		width: 33%;
	}
	p{
		text-align: center;
		font-size: 16px;
	}
	.img{
		width: 60px;
		height: 60px;
		margin: 0 auto;
	}
	.img image{
		width: 100%;
		height: 100%;
	}
	.tit1{
		display: flex;
		flex-direction:row;
		justify-content: space-around;
		align-items: center;
	}
	.img1{
		width: 30px;
		height: 30px;
		margin: 0 auto;
	}
	.img1 image{
		width: 100%;
		height: 100%;
	} */

</style>
