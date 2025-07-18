<template>
	<view>
		<view class="top" :style="'top:'+menuButtonInfo.top+'px; height:'+menuButtonInfo.height+'px; line-height:'+menuButtonInfo.height+'px;'">
			<uni-icons type="arrowleft" color="#ffffff" size="26" @click="back()"></uni-icons>
		</view>
		<view class="banner">
			<swiper class="swiper" :autoplay="autoplay" :interval="interval" :duration="duration" :circular="circular"
				:indicator-dots="indicatorDots" :indicator-color="indicatorColor"
				:indicator-active-color="indicatorActiveColor">
				<block v-for="(v, k) in banner">
					<swiper-item>
						<navigator class="swiper-item" hover-class="none">
							<image :src="v.img" mode="aspectFill"/>
						</navigator>
					</swiper-item>
				</block>
			</swiper>
		</view>
		<view class="part">
			<view class="title">
				<text class="first">{{info.name}}</text>
				<text>{{info.title}}</text>
			</view>
			<view class="content">
				<image src="../../static/image/border_l.png"></image>
				<view class="name">
					<text class="first">{{info.text}}</text>
					<text>{{info.organ}}</text>
					<text>{{info.teacher}}</text>
				</view>
				<view class="address">
					<image src="../../static/image/address2.png"></image>
					<text>{{info.address}}</text>
					<image src="../../static/image/address3.png"></image>
				</view>
				<view class="time">
					<image src="../../static/image/time.png"></image>
					<text>{{info.from}} 至 {{info.to}}</text>
				</view>
			</view>
		</view>
		<view class="sylla">
			<view class="title">教程大纲</view>
			<view class="entry">
				<block v-for="(v,k) in outline">
					<view class="row">
						<view class="left">
							0{{k+1}}
						</view>
						<view class="right">
							<view class="desc">{{v.title}}</view>
							<view class="time">
								<image src="../../static/image/time2.png"></image>
								<text>{{v.time}} 上课 {{v.teacher}}</text>
							</view>
						</view>
						<view class="clear"></view>
					</view>
				</block>
			</view>
		</view>
		<view class="bottom">
			<view class="left" @click="back()">
				<uni-icons type="arrowleft" size="20" color="#6d6e80"></uni-icons>
			</view>
			<view class="left">
				<button class="row" open-type="share">
					<view><image src="../../static/image/share.png"></image></view>
					<view>分享</view>
				</button>
			</view>
			<view class="left">
				<navigator class="row" hover-class="none" :url="'/pages/lesson/content?url='+info.video">
					<view><image src="../../static/image/listen.png"></image></view>
					<view>试听</view>
				</navigator>
			</view>
			<view class="right" @click="sign()">
				<text>报名</text>
			</view>
		</view>
	</view>
</template>

<script>
	const app = getApp()
	var that
	export default {
		data() {
			return {
				autoplay: true,
				interval: 2000,
				duration: 500,
				circular: true,
				banner: {},
				menuButtonInfo:'',
				info:'',
				id:'',
				outline:''
			}
		},
		onLoad(e) {
			that = this
			that.id = e.id
		},
		methods: {
			//banner信息
			getBanner() {
				uni.request({
					url: app.globalData.url + 'banner',
					success(res) {
						that.banner = res.data.data
					}
				})
			},
			getInfo(){
				uni.request({
					url:app.globalData.url+'course/'+that.id,
					success(res) {
						if(res.data.result == 1){
							that.info = res.data.data
							that.outline = res.data.outline
						}
					}
				})
			},
			sign(){
				if(!app.globalData.api_token){
					uni.navigateTo({
						url:'/pages/login/login'
					})
				}
				uni.showLoading()
				uni.request({
					url:app.globalData.url+'course/sign',
					method:'POST',
					data:{api_token:app.globalData.api_token,id:that.id},
					success(res) {
						uni.showToast({
							title:res.data.msg,
							icon:'none'
						})
					},complete() {
						setTimeout(function(){
							uni.hideLoading()
						},2000)
					}
				})
			},
			back(){
				uni.navigateBack({
					fail(res){
						uni.reLaunch({
							url:'/pages/index/index'
						})
					}
				})
			}
		},
		onShow() {
			that.getBanner()
			that.menuButtonInfo = uni.getMenuButtonBoundingClientRect()
			uni.setStorage({
				key: 'url',
				data: '/pages/course/detail?id='+that.id,
				success: function() {
					app.isLogin().then(function() {
						that.getInfo()
					})
				}
			})
			
		},
		onShareAppMessage(res) {
			return {
			  title: that.info.title,
			  path: '/pages/course/detail?id='+that.id,
			  imageUrl: that.info.img
			}
		}
	}
</script>

<style>
@import url('@/static/css/course.css')
</style>
