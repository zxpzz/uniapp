<template>
	<view>
		<view class="body">
			<view class="" v-for="xinwen of xinwen">
				<uni-list-item clickable="true" @click="neirong(xinwen.id)" thumbSize="lg" :title="xinwen.title" :thumb="'http://10.1.33.249:10001'+xinwen.cover"
					:rightText="`${xinwen.createTime}\n${xinwen.commentNum}评论`"></uni-list-item>
		
			</view>
			
		
		</view>
			<view v-if="xinwen.length<1">
				<p style="text-align: center; font-size: 20px;padding-top: 200px;color: red;">未找到新闻</p>
			</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				search:'',
				xinwen:[]
			}
		},
		onLoad:function(o){
			this.search=o.search
		},
		mounted() {
			console.log('sssssssssssss');
			uni.request({
				url: `${uni.getStorageSync('ip')}/prod-api/press/press/list`,
				header: {
					'Content-Type': 'application/x-www-form-urlencoded'
				},
				success: (res) => {
					console.log(res);
					res.data.rows.forEach(s=>{
						if(s.title.indexOf(this.search)!=-1){
							this.xinwen.push(s)
						}
					})
					console.log(this.xinwen);
				}
			})
		},
		methods: {
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
	.body {
		display: flex;
		flex-direction: column;
		width: 100%;
		background-color: aqua;
	
	}
	
</style>
