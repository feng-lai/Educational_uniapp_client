<template>
	<view class="content">
		<view class="header">
			<image src="../../static/shilu-login/logo.png"></image>
		</view>
		<view class="list">
			<view class="list-call">
				<image class="img" src="/static/shilu-login/1.png"></image>
				<input class="sl-input" v-model="phone" type="number" maxlength="11" placeholder="手机号"
					@input="changePhone" />
				<view class="yzm"><button size="mini" open-type="getPhoneNumber"
						@getphonenumber="getPhone">获取微信手机号</button></view>
			</view>
			<view class="list-call">
				<image class="img" src="/static/shilu-login/3.png"></image>
				<input class="sl-input" v-model="code" type="number" maxlength="4" placeholder="验证码"
					@input="changeCode" />
				<view class="yzm" :class="{ yzms: second>0 }" @tap="getcode">{{yanzhengma}}</view>
			</view>
			<uni-data-picker :localdata="gradeInfo" popup-title="请选择年级" @change="onchange">
				<view class="list-call">
					<image class="img" src="/static/shilu-login/grade.png"></image>
					<input class="sl-input" type="text" name="grade" :value="grade.text" placeholder="请选择年级"
						disabled="true" />
				</view>
			</uni-data-picker>
		</view>
		<view class="button-login" hover-class="button-hover" @click="getUserInfo">
			<text>登录</text>
		</view>
	</view>
</template>

<script>
	const app = getApp()
	var _this, js;
	export default {
		onLoad() {
			_this = this;
		},
		onShow() {
			this.getGrade()
			uni.getStorage({
				key:'url',
				success(res) {
					_this.url = res.data
				}
			})
		},
		onUnload() {
			clearInterval(js)
			this.second = 0;
		},
		data() {
			return {
				phone: '',
				code: '',
				second: 0,
				grade: '',
				gradeInfo: [],
				userInfo:'',
				url:'',
				invite_id:''
			};
		},
		computed: {
			yanzhengma() {
				if (this.second == 0) {
					return '获取验证码';
				} else {
					if (this.second < 10) {
						return '重新获取0' + this.second;
					} else {
						return '重新获取' + this.second;
					}
				}
			}
		},
		onUnload() {
			this.clear()
		},
		methods: {
			onchange(e) {
				this.grade = e.detail.value[0]
			},
			changeCode(e) {
				this.code = e.detail.value
			},
			changePhone(e) {
				this.phone = e.detail.value
			},
			getCode() {
				uni.request({
					url: app.globalData.url + 'user/code',
					data: {
						phone: this.phone
					},
					success(res) {
						uni.showToast({
							title: res.data.msg,
							icon: 'none'
						})
					}
				})
			},
			getPhone(e) {
				var that = this
				uni.getProvider({
					service: 'oauth',
					success: function(res) {
						if (~res.provider.indexOf('weixin')) {
							uni.login({
								provider: 'weixin',
								success: function(loginRes) {
									uni.request({
										url: app.globalData.url + 'user/phone',
										data: {
											code: loginRes.code,
											encryptedData: e.detail.encryptedData,
											iv: e.detail.iv,
										},
										method: 'post',
										success(res) {
											if (res.data.result == 1) {
												that.phone = res.data.data
											} else {
												uni.showToast({
													title: '系统繁忙，请稍后重试',
													icon: 'none'
												})
											}
											uni.showToast({
												title: res.data.msg,
												icon: 'none'
											})
										}
									})
								}
							});
						}
					}
				});
			},
			getUserInfo(res) {
				var that = this
				if(!that.phone || that.phone.length != 11){
					uni.showToast({
						title:'请填写正确手机号',
						icon:'none'
					})
					return;
				}
				if(!that.code){
					uni.showToast({
						title:'请填写验证码',
						icon:'none'
					})
					//return;
				}
				if(!that.grade){
					uni.showToast({
						title:'请选择所在年级',
						icon:'none'
					})
					return;
				}
				if(that.userInfo){
					that.login()
					return;
				}
				wx.getUserProfile({
					desc: '用于完善会员资料', // 声明获取用户个人信息后的用途，后续会展示在弹窗中，请谨慎填写
					success: (res) => {
						that.userInfo = res.rawData
						if (res.rawData) {
							uni.getStorage({
								key:'invite_id',
								success(e){
									_this.invite_id = e.data
								},
								complete() {
									that.login()
								}
							})
						}else{
							uni.showToast({
								title: '请允许授权',
								icon: 'none'
							})
						}
					}
				})
			},
			
			login() {
				var that = this
				wx.login({
					success(ros) {
						if (ros.code) {
							let data = JSON.parse(that.userInfo)
							data.code = ros.code
							data.phone = that.phone
							data.codes = that.code
							data.grades_id = that.grade.value
							data.invite_id = that.invite_id
							uni.request({
								url: app.globalData.url + 'login',
								method: 'post',
								data: JSON.stringify(data),
								success(res) {
									if (res.data.result == 1) {
										uni.setStorage({
											key: 'api_token',
											data: res.data.data
										});
										uni.setStorage({
											key: 'grade',
											data: _this.grade
										});
										if (that.url) {
											uni.redirectTo({
												url: that.url,
												fail() {
													uni.reLaunch({
														url: that.url
													})
												}
											})
										} else {
											uni.reLaunch({
												url: '/pages/index/index'
											})
										}
									}
									uni.showToast({
										title:res.data.msg,
										icon:'none'
									})
								}
							})
						} else {
							console.log('失败！' + res.errMsg)
						}
					}
				})
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
			clear() {
				clearInterval(js)
				js = null
				this.second = 0
			},
			display() {
				this.showPassword = !this.showPassword
			},
			agreementSuccess() {
				this.agreement = !this.agreement;
			},
			getcode() {
				if (this.phone.length != 11) {
					uni.showToast({
						icon: 'none',
						title: '手机号不正确'
					});
					return;
				}
				if (this.second > 0) {
					return;
				}
				this.getCode()
				this.second = 60;
				//请求业务
				js = setInterval(function() {
					_this.second--;
					if (_this.second == 0) {
						_this.clear()
					}
				}, 1000)
			}
		}
	}
</script>

<style>
	.header {
		width: 161rpx;
		height: 161rpx;
		background: rgba(63, 205, 235, 1);
		box-shadow: 0rpx 12rpx 13rpx 0rpx rgba(63, 205, 235, 0.47);
		border-radius: 50%;
		margin-top: 30rpx;
		margin-left: auto;
		margin-right: auto;
	}

	.header image {
		width: 161rpx;
		height: 161rpx;
		border-radius: 50%;
	}

	.list {
		display: flex;
		flex-direction: column;
		padding-top: 50rpx;
		padding-left: 70rpx;
		padding-right: 70rpx;
	}

	.list-call {
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
		height: 100rpx;
		color: #333333;
		border-bottom: 0.5px solid #e2e2e2;
	}

	.list-call .img {
		width: 40rpx;
		height: 40rpx;
	}

	.list-call .sl-input {
		flex: 1;
		text-align: left;
		font-size: 32rpx;
		margin-left: 16rpx;
	}

	.yzm {
		color: #FF7D13;
		font-size: 24rpx;
		line-height: 64rpx;
		padding-left: 10rpx;
		padding-right: 10rpx;
		width: auto;
		height: 64rpx;
		border: 1rpx solid #FFA800;
		border-radius: 50rpx;
	}

	.yzm button {
		background-color: inherit;
		border: 0;
		color: #FF7D13;
	}

	.yzm button::after {
		border: none
	}

	.yzms {
		color: #999999 !important;
		border: 1rpx solid #999999;
	}

	.button-login {
		color: #FFFFFF;
		font-size: 34rpx;
		width: 470rpx;
		height: 100rpx;
		line-height: 100rpx;
		background: linear-gradient(-90deg, rgba(63, 205, 235, 1), rgba(188, 226, 158, 1));
		box-shadow: 0rpx 0rpx 13rpx 0rpx rgba(164, 217, 228, 0.2);
		border-radius: 50rpx;
		text-align: center;
		margin-left: auto;
		margin-right: auto;
		margin-top: 100rpx;
	}

	.button-hover {
		background: linear-gradient(-90deg, rgba(63, 205, 235, 0.8), rgba(188, 226, 158, 0.8));
	}

	.agreement {
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
		font-size: 30rpx;
		margin-top: 80rpx;
		color: #FFA800;
		text-align: center;
		height: 40rpx;
		line-height: 40rpx;
	}

	.agreement image {
		width: 40rpx;
		height: 40rpx;
	}
</style>
