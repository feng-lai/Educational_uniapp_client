<template>
	<view>
		<view class="top" :style="'height:'+height+'px; padding-top:'+top+'px;'">
			<view class="title" :style="'height:'+height+'px; line-height:'+height+'px; top:'+top+'px;'">
				<text>名师</text>
				<view class="back" :style="'height:'+height+'px;'"><uni-icons type="arrowthinleft" size="26"></uni-icons></view>
			</view>
			<uni-data-picker :localdata="gradeInfo" popup-title="请选择年级" @change="onchange">
				<view class="right grade">
					<text>{{grade.text}}</text>
					<view class="img">
						<image src="../../static/image/icon1.png"></image>
					</view>
				</view>
			</uni-data-picker>
		</view>
		<view class="banner" :style="'margin-top:'+all+'px;'">
			<swiper class="swiper" :autoplay="autoplay" :interval="interval" :duration="duration" :circular="circular" :indicator-dots="indicatorDots" :indicator-color="indicatorColor" :indicator-active-color="indicatorActiveColor">
				<block v-for="(v, k) in banner">
					<swiper-item>
						<navigator class="swiper-item" hover-class="none">
							<image :src="v.img" /> 
						</navigator>
					</swiper-item>
				</block>
			</swiper>
		</view>
		<view class="entry">
			<block v-for="(v,k) in info">
				<view class="row">
					<view class="title">
						<image :src="v.video_icon"></image>
						<text :style="'color:'+v.video_color">{{v.cate}}</text>
						<navigator class="right" hover-class="none" :url="'/pages/video/video?id='+v.subjects_id">更多></navigator>
					</view>
					<view class="video">
						<block v-for="(val,key) in v.data">
							<navigator class="part" hover-class="none" :url="'/pages/video/detail?id='+val.id">
								<view class="img">
									<image :src="val.img" mode="scaleToFill"></image>
									<view class="num">1203</view>
								</view>
								<view class="titles">3天带你玩转音标</view>
								<view class="content">带你学习元音和辅音</view>
							</navigator>
						</block>
					</view>
				</view>
			</block>
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
				indicatorDots:true,
				indicatorColor:'#6edab4',
				indicatorActiveColor:'#fafafa',
				banner: {},
				height: 0,
				top: 0,
				all:0,
				grade: '',
				gradeInfo: [],
				info:[]
			}
		},
		onLoad(){
			that = this
		},
		onShareAppMessage() {
		
		},
		
		
		methods: {
			//选择年级
			onchange(e) {
				this.grade = e.detail.value[0]
				uni.setStorage({
					key: 'grade',
					data: e.detail.value[0]
				})
				//更改用户年级信息
				if(app.globalData.api_token){
					uni.request({
						url:app.globalData.url+'user/grade',
						method:'post',
						data:{api_token:app.globalData.api_token,id:e.detail.value[0].value}
					})
				}
			},
			getBanner() {
				uni.request({
					url: app.globalData.url + 'banner',
					success(res) {
						that.banner = res.data.data
					}
				})
			},
			//年级信息
			getGrade() {
				var that = this
				uni.request({
					url: app.globalData.url + 'cate/grade',
					success(res) {
						that.gradeInfo = res.data.data
						uni.getStorage({
							key: 'grade',
							success(ros) {
								that.grade = ros.data
								that.getInfo()
							},
							fail() {
								that.grade = res.data.data[0]
								that.getInfo()
								uni.setStorage({
									key: 'grade',
									data: res.data.data[0]
								})
							}
						})
					}
				})
			},
			getInfo(){
				uni.request({
					url:app.globalData.url+'video/cate',
					data:{gid:that.grade.value},
					success(res) {
						if(res.data.result == 1){
							that.info = res.data.data
						}
					}
				})
			}
		},
		onShow() {
			let menuButtonInfo = uni.getMenuButtonBoundingClientRect()
			that.height = parseInt(menuButtonInfo.height)
			that.top = parseInt(menuButtonInfo.top)
			that.all = parseInt(menuButtonInfo.height)+parseInt(menuButtonInfo.top)
			that.getBanner()
			uni.setStorage({
				key: 'url',
				data: '/pages/video/video',
				success: function() {
					app.isLogin().then(function() {
						that.getGrade()
					})
				}
			})
		}
	}
</script>

<style>
@import url('@/static/css/video.css')
</style>
