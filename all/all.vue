<template>
	<view>
		<uni-search-bar v-model="ser" cancelText="搜索" @cancel="sr"></uni-search-bar>
		<view class="body">
			<view class="bodytitle">
				<scroll-view class="serviceLeft" scroll-y="true" scroll-with-animation="true">
					<uni-list v-for="(fuwu , index) of fuwu" :key="index">
						<uni-list-item :title="fuwu" clickable="true" @click="fenlei(fuwu,index)" :class="tit==index?'c':''"></uni-list-item>
					</uni-list>
				</scroll-view>
			</view>
			<view class="bodyall">
				<view class="bodyall1" v-for="all of all" @click="go(all.link)">
					<view class="img">
						<image :src="ip+all.imgUrl"  widthFlex></image>
					</view>
					<view class="">
						<p>{{all.serviceName}}</p>
					</view>
					
				</view>
			</view>
		</view>
		
		
		<uni-popup ref="pop">
			<uni-popup-dialog content="请先登录后访问"></uni-popup-dialog>
		</uni-popup>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				all: [],
				fuwu:[],
				ip:'',
				fl:[],
				flall:[],
				ser:'',
				tit:0
			}
		},
		onShow() {
			this.ip=uni.getStorageSync('ip')
			uni.request({
				url: `${uni.getStorageSync('ip')}/prod-api/api/service/list`,
				header: {
					'Content-Type': 'application/x-www-form-urlencoded'
				},
				success: (res) => {
					this.all=[]
					console.log(this.all);
					console.log(res);
					 res.data.rows.forEach(s=>{
						if(s.serviceName=="政府服务热线"){
							s.link='fuwu/fuwu'
						}
						this.all.push(s)
					})
					this.flall=this.all
					let arr = [];
					for (let i = 0; i < this.all.length; i++) {
						if (arr.indexOf(this.all[i].serviceType) === -1) {
							arr.push(this.all[i].serviceType);
						}
					}
					this.fuwu=arr
					
					console.log(this.fuwu);
					console.log(this.all);



				}
			})
		},
		methods: {
			fenlei(id,index){
				this.all=this.flall
				this.all=this.all.filter(s=>s.serviceType===id)
				this.tit=index
				
			},
			sr(){
				this.all=this.flall
				this.all=this.all.filter(s=>s.serviceName===this.ser)
			},
			go(link){
				if(uni.getStorageSync('token')==''){
					this.$refs.pop.open()
				}else{
					if(this.all.serviceName=="政府服务热线"){
					}
					uni.navigateTo({
						url:`/pages/all/${link}`
					})
				}
			
			}

		}
	}
</script>

<style>
	.body{
		display: flex;
		justify-content: space-around;
		flex-direction: row;
		width: 100%;
	}
	.bodytitle{
		width: 30%;
	}
	.bodyall{
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		width: 70%;
	}
	.bodyall1{
		overflow: hidden;
		display: flex;
		flex-direction: column;
		text-align: center;
		padding-bottom: 30px;
		width: 30%;
		
	}
	.img{
		width: 60px;
		height: 60px;
		margin: 0 auto;
		
	}
	.img image{
		margin: 0 auto;
		width: 80%;
		height: 50px;
	}
	.bodyall1 p{
		text-align: center;
	}
	.c{
		color: red;
	}
	
	
	
	/* .serviceLeft{
		width: 32%;
		position: absolute;
		left: 0; top: 50px; bottom: 0;
	}
 */



</style>
