<template>
	<view>
		<view class="">
			<view class="">
				<view class="">
					<h1>{{neirong.title}}</h1>
				</view>
				<view class="img">
					<image :src="ip+neirong.cover" mode=""></image>

				</view>
				<view class="">
					<rich-text :nodes="neirong.content"></rich-text>
					<!-- <web-view src=""></web-view> -->

				</view>


				<view class="pl">
					<h1>精选评论</h1>
					<view class="pllist" v-for="pinglun of pinglun">
						<uni-list-item clickable="true" thumbSize="lg" :title="pinglun.userName" :note="pinglun.content"
							thumb="../../static/imgs/home_icon4_c.png" :rightText="pinglun.commentDate"></uni-list-item>
					</view>
				</view>


			</view>

		</view>
		<view class="bot">

		</view>
		<view class="reviewBox">
			<view class="writeIcon">
				<uni-icons type="compose" size="26"></uni-icons>
			</view>
			<view class="reviewInput">
				<input placeholder="说说你的看法" type="text" v-model="userpl" />
			</view>
			<view class="">
				<button @click="usergo" type="warn" size="26">提交</button>
			</view>
			<!-- <view class="reviewNum">
							<uni-icons type="chat" size="26"></uni-icons>
							<uni-badge class="rwBadge"  type="error" size="small"></uni-badge>
						</view> -->
		</view>


		<uni-popup ref="alertDialog" type="dialog">
			<uni-popup-dialog type="error" cancelText="关闭" confirmText="同意" title="提示" content="请登录后发表评论"
				@confirm="dialogConfirm" @close="dialogClose"></uni-popup-dialog>
		</uni-popup>

	</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				xinwenid: 0,
				neirong: {},
				ip: '',
				pinglun: [],
				userpl: ''
			}
		},
		onLoad: function(o) {
			this.xinwenid = o.id
			console.log(this.xinwenid);
		},
		onShow() {
			this.ip = uni.getStorageSync('ip')
			uni.request({
				url: `${uni.getStorageSync('ip')}/prod-api/press/press/${this.xinwenid}`,
				header: {
					'Content-Type': 'application/x-www-form-urlencoded'
				},
				success: (res) => {
					console.log(res);
					this.neirong = res.data.data
				}
			})


			uni.request({
				url: `${uni.getStorageSync('ip')}/prod-api/press/comments/list`,
				header: {
					'Content-Type': 'application/x-www-form-urlencoded'
				},
				success: (res) => {
					this.pinglun = res.data.rows
					console.log(this.pinglun);
				}
			})
		},
		methods: {
			usergo() {
				if (uni.getStorageSync('token') == '' || uni.getStorageSync('token') == undefined || uni.getStorageSync(
						'token')==null) {
						
							this.$refs.alertDialog.open()

				} else {
					uni.request({
						url: `${uni.getStorageSync('ip')}/prod-api/press/pressComment`,
						method: 'POST',
						header: {
							'Content-Type': 'application/json',
							Authorization: uni.getStorageSync('token')
						},
						data: {
							"newsId": this.neirong.id,
							"content": this.userpl
						},
						success: (res) => {
							console.log(res);
							this.userpl = ''
							uni.request({
								url: `${uni.getStorageSync('ip')}/prod-api/press/comments/list`,
								header: {
									'Content-Type': 'application/x-www-form-urlencoded'
								},
								success: (res) => {
									this.pinglun = res.data.rows
									console.log(this.pinglun);
								}
							})
						}
					})
				}



			}

		}
	}
</script>

<style>
	h1 {
		text-align: center;
	}

	.img {
		width: 100%;
		box-sizing: border-box;
		padding: 20px 15px;
		background-image: linear-gradient(to bottom, #FEFEFE, #E3EFF8);
		box-shadow: 0px 0px 5px #D3D3D3;
	}

	.img image {
		width: 100%;
		background: no-repeat;
	}

	.reviewBox {

		width: 100%;
		height: 50px;
		padding: 0 15px;
		box-sizing: border-box;
		background-color: #fff;
		position: fixed;
		left: 0;
		bottom: 0;
		z-index: 9;
		display: flex;
		justify-content: space-between;
		align-items: center;
		box-shadow: -3px 0 3px rgba(0, 0, 0, 0.4);
	}

	.reviewInput {
		flex: auto;
		padding-left: 8px;
	}

	.reviewNum {
		position: relative;
	}

	.rwBadge {
		position: absolute;
		left: 12px;
		top: -8px;
	}

	.pl {
		padding-top: 100px;
		width: 100%;
	}

	.pl h1 {
		text-align: center;
	}

	.pllist {
		padding-top: 30px;
	}

	.bot {
		height: 50px;
		width: 100%;
	}
</style>
