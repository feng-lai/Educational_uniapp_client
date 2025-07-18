<template>
	<view>
		<uni-list>
		    <uni-list-item  :title="v.text" :note="v.score" v-for="(v,k) in info"></uni-list-item>
		</uni-list>
		<view style="width: 100%;">
			<uni-load-more :status="more" :contentText="contentText"></uni-load-more>
		</view>
	</view>
</template>

<script>
	const app = getApp()
	var that
	export default {
		data() {
			return {
				num:10,
				page:1,
				info:[],
				more: 'loading',
				contentText: {
					contentdown: "上拉显示更多",
					contentrefresh: "正在加载...",
					contentnomore: "没有更多数据了"
				},
			}
		},
		methods: {
			getInfo(){
				uni.request({
					url:app.globalData.url+'user/score/detail',
					data:{page:that.page,num:that.num,api_token:app.globalData.api_token},
					success(res) {
						uni.setNavigationBarTitle({
						　　title:'积分明细('+res.data.score+')'
						})
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
			}
		},
		onLoad() {
			that = this
		},
		onShow() {
			uni.setStorage({
				key: 'url',
				data: '/pages/score/score',
				success: function() {
					app.isLogin().then(function() {
						app.checkLogin(1)
						that.getInfo()
					})
				}
			})
		},
		onReachBottom() {
			that.getInfo()
		}
	}
</script>

<style>

</style>
