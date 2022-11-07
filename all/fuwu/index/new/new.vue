<template>
	<view>
		<view class="">
			<uni-list>
				<uni-list-item title="所属诉求分类" clickable="true" @click="fl" :rightText="sq"></uni-list-item>
				<uni-list-item title="诉求标题" clickable="true" @click="up(1)" :rightText="title"></uni-list-item>
				<uni-list-item title="诉求内容" clickable="true" @click="up(2)" :rightText="neirong"></uni-list-item>
				<uni-list-item title="承办单位" clickable="true" @click="up(3)" :rightText="bm"></uni-list-item>
				<uni-list-item title="诉求图片"></uni-list-item>
			</uni-list>
		</view>



		<view class="" style="width: 40%; padding-top: 100px; margin: 0 auto;">
			<button type="primary" @click="go">提交诉求</button>
		</view>

		<uni-popup ref="pop">
			<uni-popup-dialog title="诉求分类" @confirm="qr">
				<view class="">
					<uni-data-checkbox v-model="rd" :localdata="feneli"></uni-data-checkbox>
				</view>
			</uni-popup-dialog>
		</uni-popup>


		<uni-popup ref="pop1">
			<uni-popup-dialog @confirm="queren">
				<uni-easyinput v-model="jicun" type="textarea" autoHeight="true" :styles="sty"></uni-easyinput>
			</uni-popup-dialog>
		</uni-popup>


		<uni-popup ref="pop2">
			<uni-popup-dialog :content="msg"></uni-popup-dialog>
		</uni-popup>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				feneli: [{}],
				news: [],
				rd: 8,
				sq: '',
				title: '',
				neirong: '',
				bm: '',
				jicun: '',
				sty: {
					color: '#2979FF',
					borderColor: '#2979FF'
				},
				panduan: '',
				msg: ''

			}
		},
		onLoad() {
			uni.request({
				url: `${uni.getStorageSync('ip')}/prod-api/api/gov-service-hotline/appeal-category/list`,
				header: {
					'Content-Type': 'application/x-www-form-urlencoded',
					Authorization: uni.getStorageSync('token')
				},
				success: (res) => {
					console.log(res);
					this.news = res.data.rows
					this.news.forEach(s => {
						let arr = {}
						arr.text = s.name
						arr.value = s.id
						this.feneli.push(arr)
					})
					this.feneli = this.feneli.filter(s => s.text != undefined)

				}
			})
		},
		methods: {
			fl() {
				this.$refs.pop.open()
			},
			qr() {
				for (let key in this.feneli) {
					if (this.feneli[key].value == this.rd) {
						this.sq = this.feneli[key].text
					}
				}
			},
			up(e) {
				this.panduan = e
				switch (this.panduan) {
					case 1:
						this.jicun = this.title
						break;
					case 2:
						this.jicun = this.neirong
						break;
					case 3:
						this.jicun = this.bm
						break;

				}
				this.$refs.pop1.open()
			},
			queren() {
				switch (this.panduan) {
					case 1:
						this.title = this.jicun
						break;
					case 2:
						this.neirong = this.jicun
						break;
					case 3:
						this.bm = this.jicun
						break;
				}
			},
			go() {
				if (this.rd == '' || this.title == '' || this.neirong == '' || this.bm == '') {
					this.msg = '反馈内容不能为空'
					this.$refs.pop2.open()
				} else {
					uni.request({
						url: `${uni.getStorageSync('ip')}/prod-api/api/gov-service-hotline/appeal`,
						method: 'POST',
						header: {
							'Content-Type': 'application/json',
							Authorization: uni.getStorageSync('token')
						},
						data: {
							"appealCategoryId": this.rd,
							"title": this.title,
							"content": this.neirong,
							"undertaker": this.bm,
							"imgUrl": "/profile/abc.png"
						},
						success: (res) => {
							console.log(res);
							this.msg = res.data.msg
							this.$refs.pop2.open()
						}
					})
				}

			}

		}
	}
</script>

<style>

</style>
