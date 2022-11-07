<template>
	<view>
		<view class="">
			<uni-list>
				<uni-list-item title="标题" :rightText="this.list.title"></uni-list-item>
				<uni-list-item title="诉求内容" >
					<template v-slot:footer>
						<view style="width: 30%;">
							<p style="color: #a6a6a6;font-size: 12px;">{{list.content}}</p>
						</view>
					</template>
				</uni-list-item>
				<uni-list-item title="图片">
					<template v-slot:footer>
						<view style="width: 50%;">
							<image style="width: 100%;" :src="ip+list.imgUrl" mode="widthFix"></image>
						</view>
					</template>
				</uni-list-item>
				<uni-list-item title="承办单位" :rightText="list.undertaker"></uni-list-item>
				<uni-list-item title="提交时间" :rightText="list.updateTime"></uni-list-item>
				<uni-list-item title="反馈处理状态处理结果" :rightText="list.state==0?'未处理':'已处理'"></uni-list-item>
				<uni-list-item title="处理结果" :rightText="list.detailResult"></uni-list-item>
			</uni-list>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				id:'',
				list:{},
				ip:''
			}
		},
		onLoad:function(o){
			this.id=o.id
			this.ip=uni.getStorageSync('ip')
		},
		onShow() {
			uni.request({
				url:`${uni.getStorageSync('ip')}/prod-api/api/gov-service-hotline/appeal/${this.id}`,
				header:{
					'Content-Type':'application/x-www-form-urlencoded',
					Authorization:uni.getStorageSync('token')
				},
				success: (res) => {
					console.log(res);
					this.list=res.data.data
					console.log(this.list);
				}
			})
		},
		methods: {
			
		}
	}
</script>

<style>

</style>
