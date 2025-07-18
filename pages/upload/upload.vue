<template>
	<view>
		<uni-list>
			<block v-for="(v,k) in info">
				<uni-list-item :title="v.score" :thumb="v.img"
		 thumb-size="lg" :rightText="v.updated_at" :note="v.text"></uni-list-item>
			</block>
		</uni-list>
		<view style="width: 100%;">
			<uni-load-more :status="more" :contentText="contentText"></uni-load-more>
		</view>
		<view  style="width: 100%; height: 100rpx;"></view>
		<view class="btn" @click="upload()">上传资料图片</view>
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
				more: 'loading',
				contentText: {
					contentdown: "上拉显示更多",
					contentrefresh: "正在加载...",
					contentnomore: "没有更多数据了"
				},
				info:[]
			}
		},
		onLoad() {
			that = this
		},
		onShow() {
			uni.setStorage({
				key: 'url',
				data: '/pages/upload/upload',
				success: function() {
					app.isLogin().then(function() {
						app.isLogin(1)
						that.getInfo()
					})
				}
			})
		},
		methods: {
			getInfo(){
				uni.request({
					url: app.globalData.url + 'user_upload_img/entry',
					data: {
						num: that.num,
						page: that.page,
						api_token:app.globalData.api_token
					},
					success(res) {
						let page = that.page;
						var content = that.info; //页码为1时
						if (page == 1) {
							content = [];
						}
						var info = res.data.data; //后台请求拿到的结果
						that.is_video = res.data.is_video
						//如果请求的数据小于20  就提示没有更多数据 否则正在加载
						that.info = content.concat(info)
						if (page <= res.data.lastPage) {
							that.page++
						} else {
							that.more = 'noMore'
						}
						if (that.info.length < that.num) {
							that.more = 'noMore'
						}
					}
				})
			},
			upload(){
				uni.chooseImage({
					success(res) {
						const tempFilePaths = res.tempFiles;
						if (tempFilePaths[0].size > 10485760) {
							uni.showToast({
								title: '文件超出10M,上传失败',
								icon: 'none'
							})
							return;
						}
						uni.showLoading()
						uni.uploadFile({
							url: app.globalData.url + 'user_upload_img?api_token='+app.globalData.api_token,
							filePath: tempFilePaths[0].path,
							name: 'img',
							success(res) {
								let info = JSON.parse(res.data)
								if (info.result == 1) {
									that.page = 1
									that.getInfo()
								}
								uni.showToast({
									title: info.msg,
									icon: 'none'
								})
							},
							complete() {
								uni.hideLoading()
							}
						});
					}
				
				});
			},
		}
	}
</script>

<style>
.btn{
	position: fixed;
	width: 100%;
	height: 100rpx;
	line-height: 100rpx;
	font-size: 32rpx;
	color: #FFFFFF;
	background-color: red;
	text-align: center;
	bottom: 0;
}
</style>
