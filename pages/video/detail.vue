<template>
	<view>
		<view class="video" v-if="vis">
			<video :src="info.video" :poster="v.img"></video>
		</view>
		<view class="content">
			<view class="title">{{info.title}}</view>
			<view class="cate"><text>{{info.name}}</text></view>
			<rich-text class="content" :nodes="info.content"></rich-text>
		</view>
	</view>
</template>

<script>
	const app = getApp()
	var that
	export default {
		data() {
			return {
				info:{},
				id:'',
				vis:''
			}
		},
		onLoad(e) {
			that = this
			that.id = e.id
			that.getInfo()
		},
		onShareAppMessage() {
			if(app.globalData.api_token){
				uni.request({
					url:app.globalData.url+'video/share',
					method:'post',
					data:{id:that.id,api_token:app.globalData.api_token},
				})
			}
			return {
				title: that.info.title,
				path: '/pages/video/detail?id='+that.id,
				imageUrl:that.info.img,
			}
		},
		methods: {
			getInfo(){
				uni.request({
					url:app.globalData.url+'video/detail',
					data:{id:that.id},
					success(res) {
						that.info = res.data.data
						that.vis = res.data.vis
						uni.setNavigationBarTitle({
							title:res.data.data.title
						})
					}
				})
			}
		}
	}
</script>

<style>
	video{
		width: 100%;
	}
	.content{
		padding: 0 20rpx;
	}
	.title{
		font-size: 38rpx;
		font-weight: bold;
		padding: 20rpx;
	}
	.cate text{
		height: 60rpx;
		line-height: 60rpx;
		background-color: #0E8A39;
		color: #FFFFFF;
		border-radius: 10rpx;
		padding: 0 10rpx;
		font-size: 30rpx;
	}
</style>
