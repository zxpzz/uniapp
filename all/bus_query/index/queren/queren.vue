<template>
	<view>
		<uni-card style="background-color: aliceblue;">
			<view class="body">
				<view class="">
					<p>{{xiangxi.first}}</p>
				</view>
				<view class="">
					<view class="img">
						<image src="../../../../../static/componentHL.png" mode=""></image>
					</view>
				</view>
				<view class="">
					<p>{{xiangxi.end}}</p>
				</view>
			</view>
		</uni-card>

		<view class="body1">
			<uni-card style="background-color: aliceblue;">
				<template>
					<view class="body2">
						<p>￥{{xiangxi.price}}元</p>
					</view>
				</template>
				<template>
					<view class="body3">
						<p>全程{{xiangxi.mileage}}km</p>
					</view>
				</template>
			</uni-card>
		</view>
		
		
		
		<view class="body1">
			<uni-card style="background-color: aliceblue;">
				<template>
					<view class="zan">
						<view class="all" v-for="zhan of zhan">
							<view class="icon">
								<uni-icons type="smallcircle-filled" size="20"></uni-icons>
							</view>
							<view class="zan1">
								<p>{{zhan}}</p>
							</view>
						</view>
					</view>
				</template>
			</uni-card>
		</view>
		
		

		<view class="butt">
			<view class="btn">
				<button @click="fanhui" type="primary">返回上一页</button>
			</view>
			<view class="btn">
				<button @click="go" type="warn">下一步</button>
			</view>

		</view>



	</view>
</template>

<script>
	export default {
		data() {
			return {
				id: '',
				xiangxi: {},
				zhan:[],
			}
		},
		onLoad: function(o) {
			this.id = o.id
		},
		onShow() {
			uni.request({
				url:`${uni.getStorageSync('ip')}/prod-api/api/bus/stop/list`,
				header:{
					'Content-Type':'application/x-www-form-urlencoded',
				},
				success: (res) => {
					this.zhan=[]
					res.data.rows.forEach(s=>{
						this.zhan.push(s.name)
					})
					console.log(this.zhan);
					
				}
			})
			uni.request({
				url: `${uni.getStorageSync('ip')}/prod-api/api/bus/line/${this.id}`,
				header: {
					'Content-Type': 'application/x-www-form-urlencoded',
					Authorization: uni.getStorageSync('token')
				},
				success: (res) => {
					console.log(res);
					this.xiangxi = res.data.data
					let i=this.zhan.findIndex(s=>s==this.xiangxi.first)
					let j=this.zhan.findIndex(s=>s==this.xiangxi.end)
					// let c=this.zhan.length
					// console.log(c);
					this.zhan=this.zhan.splice(i,i+j+1)

					console.log(this.zhan);
					console.log(i,j);
					
					
					// this.zhan=this.jicun.slice('光谷金融街','南湖大厦')
					// var a=[ 'a' , 's' , 'd' , 'f' , 'g' , 'h' ]
					// let i=a.indexOf(s=>s=='a')
					// console.log(a[0]);
					// console.log(a[0]=='a');
					// console.log(a.slice('a','f'));
					// console.log(this.zhan); 
					// console.log(typeof(this.jicun));
					// console.log(typeof(a));
					// console.log(a);
				}

			})
		},
		methods: {
			fanhui(){
				uni.navigateTo({
					url:'/pages/all/bus_query/custom_shuttle'
				})
			},
			go(){
				uni.navigateTo({
					url:`/pages/all/bus_query/index/index?id=${this.id}`
				})
			}
		}
	}
</script>

<style scoped>
	.body {
		padding-top: 10px;
		display: flex;
		flex-direction: row;
		justify-content: space-around;
		align-items: center;
	}

	.body view {
		flex-grow: 1;
	}

	.body view p {
		text-align: center;
		font-size: 15px;
	}

	.img {
		width: 60px;
		height: 60px;
		margin: 0 auto;
	}

	.img image {
		width: 100%;
		height: 100%;
		margin: 0 auto;
	}

	.body view p {
		font-size: 20px;
	}

	.body2 {
		width: 100%;
		padding-bottom: 15px;
		padding-top: 15px;
	}

	.body2 p {
		text-align: center;
		font-size: 20px;
		color: blue;
	}

	.body3 {
		width: 100%;
		padding-bottom: 15px;
		padding-top: 15px;
	}

	.body3 p {
		text-align: center;
		font-size: 16px;
		color: red;
	}

	.body1 {
		padding-top: 30px;

	}

	.btn {
		width: 50%;
		margin: 0 auto;
		padding-top: 10px;
	}
	.butt{
		padding-top: 100px;
	}
	.zan{
		display: flex;
		justify-content: space-around;
		flex-direction: row;
		align-items: center;
		
	}
	.zan1{
		flex-grow: 1;
	}
	.zan1 p{
		writing-mode: vertical-lr;
		color: red;
		padding-top: 20px;
	}
	
</style>
