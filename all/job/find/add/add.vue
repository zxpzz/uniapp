<template>
	<view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				ip:'',
			}
		},
		onShow() {
			this.ip=uni.getStorageSync('ip')
			uni.request({
				url:`${this.ip}/prod-api/api/job/resume`,
				method:'POST',
				header:{'Content-Type':'application/json',Authorization:uni.getStorageSync('token')},
				data:{
					"mostEducation": "博士",
					"education": "本科",
					"address": "北京",
					"experience": "1 年",
					"individualResume": "性格有点内向、乐观上进、有爱心并善于施教并行",
					"money": "7000",
					"positionId": 2,
				},
				success: (res) => {
					console.log(res);
				}
				
			})
			console.log(uni.getStorageSync('token'));
		},
		methods: {
			
		}
	}
</script>

<style>

</style>
