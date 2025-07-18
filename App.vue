<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		},
		globalData: {
			url: 'https://www.51dgf.cn:9001/api/v1/',
			openid: '',
			phone:'',
		},
		methods: {
			//检查是否登录
			checkLogin: function(type) {
				if (!this.globalData.api_token) {
					uni.showModal({
						content: '请先登录',
						success(res) {
							if (res.confirm) {
								uni.navigateTo({
									url: '/pages/login/login'
								});
							} else {
								if(type){
									uni.switchTab({
										url: '/pages/index/index'
									});
								}
							}
						}
		
					});
					return;
				}
			},
			isLogin() {
				var that = this
				return new Promise(function(resolve, reject) {
					wx.getStorage({
						key: 'api_token',
						success(res) {
							that.globalData.api_token = res.data
							resolve();
						},
						complete() {
							resolve();
						}
					})
				})
			},
			getPhone() {
				var that = this
				return new Promise(function(resolve, reject) {
					wx.getStorage({
						key: 'phone',
						success(res) {
							that.globalData.phone = res.data
							resolve();
						},
						complete() {
							resolve();
						}
					})
				})
			}
		}
	}
</script>
<style>
	.left{
		float: left;
	}
	.right{
		float: right;
	}
	.clear{
		both:clear;
	}
</style>
