<template>
	<view class="">

		<view class="">
			<uni-search-bar  placeholder="自定义背景色" v-model="searchs"  @cancel="search()" bgColor="#EEEEEE"
				cancelText="搜索" />
		</view>


		<swiper class="sw" :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000">
			<swiper-item v-for="img of img" :key="img.id">
				<image @click="test" class="img" :src="userip+img.advImg" mode="" style="width: 100%;" widthFix>
				</image>
			</swiper-item>
		</swiper>

		<view class="fuwuall">
			<view class="h3">
				<h3>推荐服务</h3>
			</view>

			<hr>
			<view class="fuwu">
				<view class="onefuwu" v-for="fuwu of fuwu">
					<!-- <view class=""> -->
					<view class="icon" @click="fw(fuwu.link)">
						<image mode="widthFix" :src="userip+fuwu.imgUrl"></image>
					</view>

					<p>{{fuwu.serviceName}}</p>
					<!-- </view> -->

				</view>
				<view class="onefuwu">
					<view class="icon" @click="alll">
						<image mode="widthFix" src="../../static/templateHL.png"></image>
					</view>

					<p>更多服务</p>
				</view>
			</view>

		</view>


		<view class="topic">
			<view class="fuwuall">
				<view class="h3">
					<h3>专题</h3>
				</view>
			</view>
			<view class="topicWrap">
				<view class="topicItem"  @click="neirong(tuijian.id)" v-for="tuijian of tuijian">
					<image :src="userip+tuijian.cover"></image>
					<text>{{tuijian.content}}</text>
				</view>
			</view>
		</view>

		
		<view class="sticky-box">
			<!-- 新闻分类吸顶 -->
			<scroll-view class="scroll-view_H" scroll-x="true"  scroll-with-animation="true">
				<view class="scroll-view-item_H" v-for="(fenlei,index) in fenlei" :key="index"
					>
					<text @click="go(fenlei.id,index)" :class="index==tit?'c':''" >{{fenlei.name}}</text>
				</view>
			</scroll-view>
		</view>


		<view class="body">
			<view class="" v-for="xinwen of xinwen">
				<uni-list-item clickable="true" @click="neirong(xinwen.id)" thumbSize="lg" :title="xinwen.title" :thumb="'http://10.1.33.249:10001'+xinwen.cover"
					:rightText="`${xinwen.createTime}\n${xinwen.commentNum}评论`"></uni-list-item>

			</view>

		</view>

	</view>


</template>

<script>
	export default {
		data() {
			return {
				img: [],
				course: [],
				searchs: '',
				all: [],
				fuwu: [],
				fenlei: [],
				xinwen: [],
				tuijian: [],
				userip: '',
				tit:0


			}
		},
		mounted() {
				this.userip = uni.getStorageSync('ip')
			uni.request({
				url: 'http://10.1.33.249:10001/prod-api/api/rotation/list',
				header: {
					'Content-Type': 'application/x-www-form-urlencoded'
				},
				success: (res) => {
					this.img = res.data.rows
					// this.img = res.data.rows
					console.log(res.data.rows);
					console.log(this.img);

				}
			})

			uni.request({
				url: 'http://10.1.33.249:10001/prod-api/api/service/list',
				header: {
					'Content-Type': 'application/x-www-form-urlencoded'
				},
				success: (res) => {
					res.data.rows.filter(s => s.sort != null && s.sort <= 9).forEach(s => {
						this.fuwu.push(s)

					})

					console.log(this.fuwu);
				}
			})

			uni.request({
				url: "http://10.1.33.249:10001/prod-api/press/category/list",
				header: {
					'Content-Type': 'application/x-www-form-urlencoded'
				},
				success: (res) => {

					this.fenlei = res.data.data
					console.log(res.data.data);
				}
			})
			uni.request({
				url: `${uni.getStorageSync('ip')}/prod-api/press/press/list`,
				header: {
					'Content-Type': 'application/x-www-form-urlencoded'
				},
				success: (res) => {
					this.tuijian = res.data.rows.filter(h => h.readNum >= 16000)
					this.xinwen = res.data.rows
					console.log(this.tuijian);
					console.log(res.data);
				}
			})

			
		},
		methods: {
			
			go(id,index){
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
				this.tit=index
			},
			neirong(id){
				console.log(id);
				uni.navigateTo({
					url:`/pages/neirong/neirong?id=${id}`
				})
			},
			
			
			nen() {

			},
			search() {
				uni.navigateTo({
					url:`search/search?search=${this.searchs}`
				})
			},
			fw(link){
				uni.navigateTo({
					url:`../all/${link}`
				})
			},
			alll(){
				uni.switchTab({
					url:'../all/all'
				})
			},
			test(){
				uni.navigateTo({
					url:'../yd/yd'
				})
			}

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

	.fuwuall {
		padding-top: 30px;
		width: 100%;
		background-color: azure;
		display: flex;
		flex-direction: column;
	}

	.h3 {
		width: 30%;
		margin: 0 auto;
	}

	.h3 h3 {
		text-align: center;
	}

	.fuwu h3 {}

	.fuwu {

		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		justify-content: center;
		width: 100%;

	}

	.onefuwu {
		overflow: hidden;
		margin-bottom: 15px;
		box-shadow: 0px 0px 5px #D3D3D3;
		width: 20%;
		display: flex;
		flex-direction: column;
		text-align: center;
	}

	.icon {
		margin: 0 auto;
		width: 50px;
		height: 50px;
		display: flex;
		align-items: center;
	}

	.icon image {
		width: 80%;
		margin: 0 auto;
	}

	.onefuwu p {
		text-align: center;
	}

	.fenlei {
		background-color: aqua;
		display: flex;
		flex-direction: row;
		justify-content: space-around;
		align-items: center;
	}

	.title {
		flex-grow: 1;
	}

	.title p {
		text-align: center;
	}

	.fenleititle {
		padding-top: 20px;
		width: 100%;
	}

	.body {
		display: flex;
		flex-direction: column;
		width: 100%;
		background-color: aqua;

	}

	.topic {
		width: calc(100% - 30px);
		margin: 0 auto;
		overflow: hidden;
	}

	.topic>h3 {
		font-size: 14px;
		line-height: 40px;
		color: #666;
	}

	.topicWrap {
		padding-top: 20px;
		display: flex;
		flex-wrap: wrap;
		justify-content: space-between;
	}

	.topicItem {
		width: calc(50% - 8px);
		background-color: #fff;
		border-radius: 10px;
		overflow: hidden;
		margin-bottom: 15px;
		box-shadow: 0px 0px 5px #D3D3D3;
	}

	.topicItem image {
		width: 100%;
		height: 90px;
	}

	.topicItem text {
		width: calc(100% - 10px);
		margin: 0 auto;
		padding-bottom: 5px;
		display: block;
		font-size: 12px;
		line-height: 20px;
		color: #3B4144;
		text-indent: 1em;
		text-overflow: -o-ellipsis-lastline;
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-line-clamp: 2;
		line-clamp: 2;
		-webkit-box-orient: vertical;
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
	.c{
		color: red;
	}












	/*  */


	/* .rec {
		display: flex;
		flex-direction: column;
		padding-bottom: 20px;
	}

	navigator image {
		height: 50px;
		width: 50px;
	}

	.recommend {
		display: flex;
		flex-direction: row;
		justify-content: space-around;
	}

	navigator {
		align-items: center;
		justify-content: center;
		display: flex;
		flex-direction: column;

	}
	
	.body1 {
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: space-around;
		border-bottom: 1px solid black;
	}

	.body1 view {
		width: 35%;
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	.body1 view text {
		text-align: center;
	} */
</style>
