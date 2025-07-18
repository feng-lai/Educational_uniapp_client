<template>
	<view>
		<view class="title">{{info.title}}</view>
		<view class="cate"><text>{{info.name}}</text></view>
		<rich-text class="content" :nodes="info.content"></rich-text>
	</view>
</template>

<script>
	const app = getApp()
	var that
	export default {
		data() {
			return {
				info:{}
			}
		},
		onLoad(e) {
			that = this
			that.id = e.id
			that.getInfo()
		},
		onShareAppMessage() {
		
		},
		methods: {
			getInfo(){
				uni.request({
					url:app.globalData.url+'information/'+that.id,
					success(res) {
						that.info = res.data.data
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
	view{
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
