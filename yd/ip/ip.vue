<template>

	<view>
		<view style="padding-top: 30px;padding-bottom: 30px;">
			<p style="text-align: center;font-size: 20px;color: red;">请设置IP</p>
		</view>
		<view style="width: 80%; margin: 0 auto;">
			<uni-easyinput v-model="input" placeholder="格式如10.1.33.249:10001" :styles="sty" />
		</view>




		<view style="width: 40%; margin: 0 auto;padding-top: 150px;">
			<button type="primary" @click="tianjia">确认添加</button>
		</view>
		
		
		<uni-popup ref="pop">
			<uni-popup-dialog :content="content" @confirm="but" :confirmText="confirmText" ></uni-popup-dialog>
		</uni-popup>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				sty: {
					color: '#2979FF',
					borderColor: '#2979FF'
				},
				input: '',
				content:'添加成功',
				confirmText:'立即体验',
				i:false
			}
		},
		methods: {
			tianjia() {
				
				this.i=/^(\d{1,2}|1\d{1,2}|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d{1,2}|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d{1,2}|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d{1,2}|2[0-4]\d|25[0-5])\:(\d|\d\d|\d\d\d|\d\d\d\d|\d\d\d\d\d)$/.test(this.input)
				if(this.i){
					this.content='添加成功',
					this.confirmText='立即体验'
					uni.removeStorageSync('ip')
					uni.setStorageSync('ip',`http://${this.input}`)
					this.$refs.pop.open()
					
				}else{
					this.content='您的IP地址格式不正确'
					this.confirmText='去修改'
					this.$refs.pop.open()
				}

			},
			but(){
				if(this.i){
					uni.switchTab({
						url:'../../index/index'
					})
				}else{
					this.$refs.pop.close()
				}
			}
		}
	}
</script>

<style>

</style>
