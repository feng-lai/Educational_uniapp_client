<template>
	<view>
		<view class="entry">
			<block v-for="(v,k) in information">
				<navigator class="row" hover-class="none" :url="'/pages/information/detail?id='+v.id">
					<view class="left">
						<image :src="v.img"></image>
					</view>
					<view class="right">
						<view class="title"><text>{{v.name}}</text>{{v.title}}</view>
						<view class="bottom">
							<view class="left">{{v.time}}</view>
							<view class="right">{{v.origin}}</view>
							<view class="clear"></view>
						</view>
					</view>
					<view class="clear"></view>
				</navigator>
			</block>
			<view style="width: 100%;">
				<uni-load-more :status="more" :contentText="contentText"></uni-load-more>
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
				page:1,
				num:10,
				information:[],
				more: 'loading',
				contentText: {
					contentdown: "上拉显示更多",
					contentrefresh: "正在加载...",
					contentnomore: "没有更多数据了"
				},
			}
		},
		onLoad(e) {
			that = this
		},
		methods: {
			//资讯
			getInformation(){
				uni.request({
					url: app.globalData.url + 'information/entry',
					data: {
						num: that.num,
						page: that.page
					},
					success(res) {
						let page = that.page;
						var content = that.information; //页码为1时
						if (page == 1) {
							content = [];
						}
						var info = res.data.data; //后台请求拿到的结果
						that.is_video = res.data.is_video
						//如果请求的数据小于20  就提示没有更多数据 否则正在加载
						that.information = content.concat(info)
						if (page <= res.data.lastPage) {
							that.page++
						} else {
							that.more = 'noMore'
						}
						if (that.information.length < that.num) {
							that.more = 'noMore'
						}
					}
				})
			}
		},
		onShareAppMessage() {
		
		},
		onReachBottom() {
			that.getInformation()
		},
		onShow() {
			uni.setStorage({
				key: 'url',
				data: '/pages/information/information',
				success: function() {
					app.isLogin().then(function() {
						
						that.getInformation()
					})
				}
			})
		}
	}
</script>

<style>
.entry {
	padding: 0 40rpx;
	margin-top: 30rpx;
}

.entry .row {
	height: 150rpx;
	margin-bottom: 30rpx;
}

.entry image {
	width: 264rpx;
	height: 150rpx;
	border-radius: 10rpx;
}
.entry .row>.left {
	width: 264rpx;
	height: 150rpx;
}

.entry .row>.right {
	width: 386rpx;
	height: 150rpx;
}

.entry .row>.right .title {
	display: -webkit-box;
	-webkit-box-orient: vertical;
	-webkit-line-clamp: 3;
	overflow: hidden;
	font-size: 24rpx;
	color:#434343;
	line-height: 36rpx;
}
.entry .row>.right .title text{
	display: inline-block;
	height: 36rpx;
	padding:0 8rpx;
	background-color: #ff4040;
	color: #FFFFFF;
	line-height: 36rpx;
	border-radius: 5rpx;
	margin-right: 6rpx;
}
.entry .row>.right{
	position: relative;
}
.entry .row>.right .bottom{
	position: absolute;
	bottom: 0;
	width: 386rpx;
	left: 0;
	font-size: 20rpx;
	color:#c6c6c6;
	line-height: 34rpx;
}
.entry .row>.right .bottom .right{
	height: 34rpx;
	padding: 0 9rpx;
	background-color: #ffefef;
	color: #ff6565;
	line-height: 34rpx;
	border-radius: 5rpx;
}
</style>
