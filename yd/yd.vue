<template>
	<view class="startUp">
		<swiper class="swiper" :indicator-dots="true" :autoplay="fales" :interval="3000" :duration="1000"
			@change="fenye" indicator-color="blue" indicator-active-color="white">
			<swiper-item v-for="img in img">
				 <view class="swiper-item">
					<view class="swiper-item-img">
						<image :src="img.u" mode="scaleToFill"></image>
					</view>
				</view>
			</swiper-item>
		</swiper>
		<view v-show="panduan" class="jump-over">
			<view style="padding-top: 30px;" @click="duankou">
				<p>设置端口</p>
			</view>
		</view>
		<view v-show="panduan" class="experience" @tap="but">立即体验</view>
		<uni-popup ref="pop">
			<uni-popup-dialog content="请先设置IP" @confirm="go"></uni-popup-dialog>
		</uni-popup>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				img: [{
						u: '../../static/imgs/splash3.png'
					},
					{
						u: '../../static/imgs/splash4.png'
					},
					{
						u: '../../static/imgs/splash2.png'
					},
					{
						u: '../../static/imgs/splash1.png'
					}
				],
				panduan: false
			}
		},
		methods: {
			fenye(e) {
				console.log(e.detail.current);
				this.panduan = e.detail.current == 3 ? true : false
				console.log(this.panduan);
			},
			duankou(){
				uni.navigateTo({
					url:'ip/ip'
				})
			},
			but(){
				if(uni.getStorageSync('ip').length>1){
					uni.switchTab({
						url:'../index/index'
					})
				}else{
					this.$refs.pop.open()
				}
			},
			go(){
				uni.navigateTo({
					url:'ip/ip'
				})
			}
		}
	}
</script>

<style scoped>
	.startUp{
		width: 100%;
		position: fixed;
		left: 0; top: 0; bottom: 0;
	}
	.startUp {
		width: 100%;
		position: fixed;
		left: 0;
		top: 0;
		bottom: 0;
	}

	.swiper {
		width: 100%;
		height: 100%;
		background: #000;
	}

	.swiper-item {
		width: 100%;
		height: 100%;
		text-align: center;
		position: relative;
		display: flex;
		align-items: flex-end;
		flex-direction: column-reverse
	}

	.swiper-item-img {
		width: 100%;
	}

	.swiper-item-img image {
		width: 100%;
		height: 100vh;
	}

	.jump-over,
	.experience {
		position: absolute;
		height: 60upx;
		line-height: 60upx;
		padding: 0 40upx;
		font-size: 32upx;
		color: #007AFF;
		z-index: 99;
	}

	.jump-over {
		right: 30upx;
		top: 80upx;
	}
	.experience{
		
		bottom: 50px;
		color: #fff;
		border: 1px solid #fff;
		border-radius: 30upx;
		right: 50%;
		margin-right: -105upx;
	}
		
</style>
