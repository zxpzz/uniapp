<template>
	<view>
		<view class="im">
			<swiper :indicator-dots="true" :autoplay="true" :interval="9000" :duration="3000">
				<swiper-item v-for="img of img" style="border-radius: 5%;">
					<image :src="ip+img.imgUrl" style="width: 100%;" mode=""></image>
				</swiper-item>
			</swiper>
		</view>
		
		
		<view class="" style="width: 90%; margin: 0 auto;padding-top: 30px;" >
			<scroll-view scroll-x="true" class="suqiu">
				<view class="tit">
					<view class="tit1" v-for="fenlei1 of fenlei1">
						<view class="tit2" @click="xiangxi(fenlei1.id)">
							<view class="img">
									<image :src="ip+fenlei1.imgUrl" mode=""></image>
								
							</view>
							<view class="text">
								<p>{{fenlei1.name}}</p>
							</view>
						</view>
					</view>
				</view>
				<br>
				<view class="tit">
					<view class="tit1" v-for="fenlei2 of fenlei2">
						<view class="tit2" @click="xiangxi(fenlei1.id)">
							<view class="img">
								 <image :src="ip+fenlei2.imgUrl" mode=""></image>
							</view>
							<view class="text">
								<p>{{fenlei2.name}}</p>
							</view>
						</view>
					</view>
					
					<view class="tit11">
						<view class="tit21" @click="news">
							<view class="img1">
								<image src="../../../static/imgs/templateHL.png" mode=""></image>
							</view>
							<view class="text1">
								<p>更多服务</p>
							</view>
						</view>
					</view>
				</view>
			</scroll-view>
		</view>
		
		<view class="mesuq">
			<view style="width: 100%;">
				<p style="text-align: center;font-size: 20px;padding-bottom: 10px;">我的诉求</p>
			</view>
			<uni-list v-for="list of list">
				<uni-list-item :title="list.title" :note="list.undertaker" :rightText="`${list.createTime}\n${list.state==0?'未解决':'已解决'}`"></uni-list-item>
			</uni-list>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				ip:'',
				img:[],
				fenlei1:[],
				fenlei2:[],
				list:[]
			}
		},
		onLoad(){
			this.ip=uni.getStorageSync('ip')
				uni.request({
					url:`${this.ip}/prod-api/api/gov-service-hotline/ad-banner/list`,
					header:{
						'Content-Type':'application/x-www-form-urlencoded',
						Authorization:uni.getStorageSync('token')
					},
					success: (res) => {
						console.log(res);
						this.img=res.data.data
						console.log(uni.getStorageSync('token'));
					}
				})
				uni.request({
					url:`${this.ip}/prod-api/api/gov-service-hotline/appeal-category/list`,
					header:{
						'Content-Type':'application/x-www-form-urlencoded',
						Authorization:uni.getStorageSync('token')
					},
					success: (res) => {
						console.log(res);
						this.fenlei1=res.data.rows.filter(s=>s.sort<=9)
						this.fenlei2=res.data.rows.filter(s=>s.sort>9)
						console.log(this.fenlei2);
					}
				})
				uni.request({
					url:`${this.ip}/prod-api/api/gov-service-hotline/appeal/my-list`,
					header:{
						'Content-Type':'application/x-www-form-urlencoded',
						Authorization:uni.getStorageSync('token')
					},
					success: (res) => {
						console.log(res);
						this.list=res.data.rows
					}
					
				})
		},
		methods: {
			xiangxi(id){
				uni.navigateTo({
					url:`index/index?id=${id}`
				})
			},
			news(){
				uni.navigateTo({
					url:'index/new/new'
				})
			}
		}
	}
</script>

<style scoped>
	.im{
		width: 90%;
		margin: 0 auto;
		padding-top: 30px;
		
	}
	.suqiu{
			/* 文本不会换行，文本会在在同一行上继续，直到遇到 <br> 标签为止。 */
			white-space: nowrap;
			width: 100%;
	}
	.suqi::-webkit-scrollbar{
		display: none !important;
	}
	.tit{
		display: flex;
		justify-content: space-around;
		flex-direction: row;
		align-items: center;
		width: 100%;
		
	}
	.tit1{
		width: 25%;
	}
	.tit2{
		width: 160px;
		display: flex;
		flex-direction: column;
	}
	.img{
		display: flex;
		justify-content: center;
		align-items: center;
		border-radius: 100%;
		width: 60px;
		height: 60px;
		margin: 0 auto;
		background-color: #00FFFF;
	}
	
	.img image{
		margin: 0 auto !important;
		width: 60%;
		height: 60%;
		text-align: center;
		
	}
	.text p{
		text-align: center;
		font-size: 12px;
	}
	
	
	
	.tit11{
		width: 1px;
	}
	.tit21{
		width: 160px;
	}
	.img1{
		display: flex;
		justify-content: center;
		align-items: center;
		border-radius: 100%;
		width: 60px;
		height: 60px;
		margin: 0 auto;
		background-color: #00FFFF;
	}
	.img1 image{
		width: 60%;
		height: 60%;
		margin: 0 auto
	}
	.text1 p{
		text-align: center;
		font-size: 12px;
	}
	.mesuq{
		padding-top: 30px;
	}

</style>
