<template>
	<view>
		<button type="default" @click="test">test</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				img:''
			}
		},
		methods: {
			test(){
				uni.chooseImage({
					count:1,
					sizeType:['compressed'],
					sourceType:['album'],
					success(res){
						console.log(typeof(res.tempFilePaths[0]));
						let arr=new Uint8Array(res.tempFilePaths[0])
						console.log(arr);
						let image=uni.arrayBufferToBase64(arr)
						console.log(image);
						// this.img=res.tempFilePaths[0]
						// uni.getFileSystemManager().readFile({
						// 	filePath:this.img,
						// 	encoding:'base64',
						// 	success:r=>{
						// 		console.log(r);
						// 	}
						// })
					}
					
				})
			}
		}
	}
</script>

<style>

</style>
