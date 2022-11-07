<template>
	<view>
		<view class="" v-if="list.length>0">
			<uni-list v-for="list of list">
				<uni-list-item clickable="true"  @click="go(list.id)" :title="list.title" :note="list.undertaker" :rightText="`${list.createTime}\n${list.state==0?'未解决':'已解决'}`"></uni-list-item>
			</uni-list>
		</view>
		<view class="kong" v-if="list.length==0" style="width: 100%;padding-top: 50px;">
			<p style="text-align: center;font-size: 20px;">暂无诉求</p>
		</view>
		
		
		<view class="btn">
			<button type="primary">发布诉求</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				id:'',
				list:[]
			}
		},
		onLoad:function(o){
			this.id=o.id
			
			
		},
		mounted() {
			console.log(this.id);
			uni.request({
				url:`${uni.getStorageSync('ip')}/prod-api/api/gov-service-hotline/appeal/list?appealCategoryId=${this.id}`,
				header:{
					'Content-Type':'application/x-www-form-urlencoded',
					Authorization:uni.getStorageSync('token')
				},
				success: (res) => {
					console.log(res);
					this.list=res.data.rows
					console.log(this.list.length);
					
				}
			})
		},
		methods: {
			go(id){
				uni.navigateTo({
					url:`xiangqing/xiangqing?id=${id}`
				})
			}
		}
	}
</script>

<style scoped>
	.btn{
		width: 40%;
		margin: 0 auto;
		padding-top: 150px;
	}
</style>
