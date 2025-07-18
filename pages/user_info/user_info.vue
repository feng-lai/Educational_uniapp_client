<template>
	<view>
		<!--pages/mine/info/index.wxml-->
		<view class="info">
			<form @submit="formSubmit">
				<view class="step">
					<view class="user">
						<view class="left">我的头像</view>
						<view class="right">
							<image :src="info.profile_pic" @tap="uploadImg"></image>
							<input type="text" :value="info.img" name="img"></input>
							<input type="text" :value="info.grades_id" name="grades_id"></input>
						</view>
						<view class="clear"></view>
					</view>
					<view class="row">
						<view class="left">昵称</view>
						<view class="right">
							<input type="text" :value="info.name" name="name" placeholder="请输入昵称" data-type="name"
								@input="change"></input>
						</view>
						<view class="clear"></view>
					</view>
				</view>
				<view class="step">
					<view class="row">
						<view class="left">手机</view>
						<view class="right">
							<input type="text" :value="info.phone" name="phone" placeholder="请输入手机号码" data-type="phone"
								@input="change"></input>
						</view>
						<view class="clear"></view>
					</view>
					<view class="row">
						<view class="left">年级</view>
						<view class="right">
							<uni-data-picker :localdata="gradeInfo" popup-title="请选择年级" @change="onchange">
								<input type="text" :value="info.grade" placeholder="请选择年级"
									disabled="true" />
							</uni-data-picker>
						</view>
						<view class="clear"></view>
					</view>
				</view>
				<button class="submit" formType="submit">确定</button>
			</form>
		</view>
	</view>
</template>

<script>
	// pages/mine/info/index.js
	const app = getApp();
	var that
	export default {
		data() {
			return {
				//图片链接
				imgUrl: app.globalData.imgUrl,
				//用户信息
				info: '',
				gradeInfo: ''
			};
		},
		onLoad() {
			that = this
		},
		/**
		 * 生命周期函数--监听页面初次渲染完成
		 */
		onShow: function() {
			var pages = getCurrentPages()
			var page = pages[pages.length - 1];
			var that = this; //是否已登录
			uni.setStorage({
				key: 'url',
				data: '/' + page.route,
				success: function() {
					app.isLogin().then(function() {
						app.checkLogin(1)
						that.getInfo()
						that.getGrade()
					})
				}
			})
		},
		methods: {
			onchange(e) {
				that.info.grade = e.detail.value[0].text
				that.info.grades_id = e.detail.value[0].value
			},

			getGrade() {
				var that = this
				uni.request({
					url: app.globalData.url + 'cate/grade',
					success(res) {
						that.gradeInfo = res.data.data
					}
				})
			},
			getInfo() {
				var that = this
				uni.request({
					url: app.globalData.url + 'user/info',
					data: {
						api_token: app.globalData.api_token
					},
					success: function(res) {
						if (res.data.result == 1) {
							that.info = res.data.data
						}
					}
				});
			},
			/**
			 * 数据提交
			 */
			formSubmit: function(e) {
				var that = this;
				let info = e.detail.value;
				info.api_token = app.globalData.api_token;
				if(!info.name){
					uni.showToast({
						title:'昵称不能为空',
						icon:'none'
					})
					return;
				}
				if(!info.phone){
					uni.showToast({
						title:'手机号码不能为空',
						icon:'none'
					})
					return;
				}
				uni.request({
					url: app.globalData.url + 'user/info/save',
					data: e.detail.value,
					method: 'post',
					success: function(res) {
						uni.showToast({
							title: res.data.msg,
							icon: 'none',
							duration: 2000
						});
						that.getInfo()
					}
				});
			},
			/**
			 * 关闭国家，性别选择
			 */
			close: function() {
				this.countryDisplay = 'countryDisplayNone'
			},
			/**
			 * 头像更换
			 */
			uploadImg: function() {
				var that = this;
				uni.chooseImage({
					success(res) {
						const tempFilePaths = res.tempFilePaths;
						uni.uploadFile({
							url: app.globalData.url + 'user/profile_pic',
							filePath: tempFilePaths[0],
							name: 'img',
							success(res) {
								let data = that.info;
								let info = JSON.parse(res.data);
								console.log(info)
								if (info.result == 1) {
									that.info.profile_pic = info.data.src; //do something
									that.info.img = info.data.img
								}
							}

						});
					}

				});
			},

			/**
			 * input值改变
			 */
			change: function(e) {
				var that = this;
				let type = e.currentTarget.dataset.type;

				if (type == 'name') {
					data.name = e.detail.value;
					that.info = data
				}


				if (type == 'phone') {
					data.phone = e.detail.value;
					that.info = data
				}
			},
		}
	};
</script>
<style>
	/* pages/mine/info/index.wxss */
	page {
		background-color: #f8f8f8;
	}

	.info .step {
		margin-bottom: 11rpx;
		background-color: #ffffff;
	}

	.info .step>.user {
		height: 166rpx;
		line-height: 166rpx;
		padding: 0 20rpx;
	}

	.info .step>.user>.left {
		color: #333333;
		font-size: 30rpx;
	}

	.info .step>.user>.right>image {
		width: 117rpx;
		height: 117rpx;
		vertical-align: middle;
		margin-right: 16rpx;
		border-radius: 117rpx;
	}

	.info .step>.user>.right>input {
		display: none;
	}

	.info .step>.row {
		height: 97rpx;
		line-height: 97rpx;
		padding: 0 20rpx;
		color: #333333;
		font-size: 30rpx;
		border-top: 1rpx solid #f8f8f8;
	}

	.info .step>.row>input {
		display: none;
		font-size: 30rpx;
		color: #333333;
	}

	.info .step>.row>.right {
		color: #a5a5a5;
	}

	.info .step>.row>.right input {
		margin-right: 16rpx;
		height: 97rpx;
		line-height: 97rpx;
		float: left;
		width: 500rpx;
		text-align: right;
	}

	.info button {
		background-color: #fa496e;
		color: #ffffff;
		width: 100%;
		margin-top: 20rpx;
	}

	.country {
		position: fixed;
		z-index: 3;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
	}

	.country>.bg {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background-color: #000000;
		opacity: 0.7;
	}

	.country>.content {
		background-color: #ffffff;
		width: 500rpx;
		height: 1000rpx;
		position: absolute;
		left: 50%;
		top: 50%;
		margin-left: -250rpx;
		margin-top: -500rpx;
		overflow: auto;
	}

	.country>.content>.text {
		height: 112rpx;
		line-height: 112rpx;
		padding: 0 19rpx;
		border-bottom: 2rpx solid #cecece;
		color: #333333;
		font-size: 30rpx;
	}

	.country>.content>.text:hover {
		background-color: #cecece;
		color: #ffffff;
	}

	.countryDisplayNone {
		display: none;
	}

	.countryDisplayBlock {
		display: block;
	}

	.country>.content>input {
		height: 112rpx;
		line-height: 112rpx;
		padding: 0 19rpx;
		width: 80%;
		margin: 0 auto;
		border-bottom: 2rpx solid #cecece;
		font-size: 30rpx;
	}

	.gender {
		position: fixed;
		z-index: 3;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
	}

	.gender>.bg {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background-color: #000000;
		opacity: 0.7;
	}

	.gender>.content {
		background-color: #ffffff;
		width: 500rpx;
		height: 226rpx;
		position: absolute;
		left: 50%;
		top: 50%;
		margin-left: -250rpx;
		margin-top: -113rpx;
		overflow: auto;
	}

	.gender>.content>.text {
		height: 112rpx;
		line-height: 112rpx;
		padding: 0 19rpx;
		border-bottom: 2rpx solid #cecece;
		color: #333333;
		font-size: 30rpx;
	}

	.gender>.content>.text:hover {
		background-color: #cecece;
		color: #ffffff;
	}

	.genderDisplayNone {
		display: none;
	}

	.genderDisplayBlock {
		display: block;
	}
</style>
