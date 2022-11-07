<template>
	<view>
		<swiper class="sw" :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000">
			<swiper-item v-for="img of img" :key="img.id">
				<image class="img" :src="ip+img.advImg" mode="" style="width: 100%;" widthFix>
				</image>
			</swiper-item>
		</swiper>
		
		
		
		<view class="sticky-box">
			<!-- 新闻分类吸顶 -->
			<scroll-view class="scroll-view_H" scroll-x="true" :scroll-left="scrollL" scroll-with-animation="true">
				<view class="scroll-view-item_H" v-for="(fenlei,index) in fenlei" :key="index"
					>
					<text @click="go(fenlei.id,index)" :class="index==tit?'c':''">{{fenlei.name}}</text>
				</view>
			</scroll-view>
		</view>
		
		
		<view class="body">
			<view class="" v-for="xinwen of xinwen">
				<uni-list-item clickable="true" @click="neirong(xinwen.id)" thumbSize="lg" :title="xinwen.title" :thumb="ip+xinwen.cover"
					:rightText="`${xinwen.createTime}\n${xinwen.commentNum}评论`"></uni-list-item>
		
			</view>
		
		</view>
		
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				ip:'',
				img:[],
				fenlei:[],
				xinwen:[],
				tit:''
				
			}
		},
		onShow() {
			this.ip=uni.getStorageSync('ip')
			uni.request({
				url: `${uni.getStorageSync('ip')}/prod-api/api/rotation/list`,
				header: {
					'Content-Type': 'application/x-www-form-urlencoded'
				},
				success: (res) => {
					console.log(res);
					this.img=res.data.rows
			
				}
			})
			uni.request({
				url:`${uni.getStorageSync('ip')}/prod-api/press/category/list` ,
				header: {
					'Content-Type': 'application/x-www-form-urlencoded'
				},
				success: (res) => {
			
					this.fenlei = res.data.data
					console.log(this.fenlei);
				}
			})
			uni.request({
				url: `${uni.getStorageSync('ip')}/prod-api/press/press/list`,
				header: {
					'Content-Type': 'application/x-www-form-urlencoded'
				},
				success: (res) => {
					this.xinwen = res.data.rows
					console.log(res.data);
				}
			})
		},
		methods: {
			go(id,i){
				this.tit=i
				uni.request({
					url: `${uni.getStorageSync('ip')}/prod-api/press/press/list`,
					header: {
						'Content-Type': 'application/x-www-form-urlencoded'
					},
					data:{
						type:id
					},
					success: (res) => {
						console.log(res);
						this.xinwen=res.data.rows
						
					}
				})
				
			},
			neirong(id){
				console.log(id);
				uni.navigateTo({
					url:`/pages/neirong/neirong?id=${id}`
				})
			},
			
		}
	}
</script>

<style>
	.sw {
		width: 90%;
		margin: 0 auto;
		border-radius: 100%;
		height: 240px;
	}

	.img {
		border-radius: 5%;
	}
	.sticky-box {
		/* #ifndef APP-PLUS-NVUE */
		display: flex;
		position: -webkit-sticky;
		/* #endif */
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
	
	.textcenter {
		text-align: center;
		font-size: 14px;
	}
	
	.scroll-view_H {
		/* 文本不会换行，文本会在在同一行上继续，直到遇到 <br> 标签为止。 */
		white-space: nowrap;
		width: 100%;
	}
	
	.scroll-view_H::-webkit-scrollbar {
		display: none !important;
	}
	
	.scroll-view-item_H {
		display: inline-block;
		padding: 0 30upx;
		margin: 2% 0;
		border-right: 1upx #e2e2e2 solid;
	}
	
	.scroll-view-item_H view {
		box-sizing: border-box;
	}
	
	.scItemActive {
		color: red;
	}
	
	.body {
		display: flex;
		flex-direction: column;
		width: 100%;
		background-color: aqua;
	
	}
	.c{
		color: red;
	}

</style>
