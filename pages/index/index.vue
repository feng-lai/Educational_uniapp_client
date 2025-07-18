<template>
	<view>
		<view class="top" :style="'height:'+height+'px; padding-top:'+top+'px;'">
			<view class="left title"><text>首页</text></view>
			<view class="left line"><text>|</text></view>
			<view class="left address" @click="choseAddress">
				<view class="img">
					<image src="../../static/image/address.png"></image>
				</view>
				<text>{{provice}}</text>
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
			<swiper class="swiper" :autoplay="autoplay" :interval="interval" :duration="duration" :circular="circular"
				:indicator-dots="indicatorDots" :indicator-color="indicatorColor"
				:indicator-active-color="indicatorActiveColor">
				<block v-for="(v, k) in banner">
					<swiper-item>
						<navigator class="swiper-item" hover-class="none" :url="'/pages/banner/banner?url='+v.url" v-if="v.type==1">
							<image :src="v.img" />
						</navigator>
						<navigator class="swiper-item" hover-class="none" target="miniProgram" :app-id="v.appid" :url="'/pages/banner/banner?url='+v.url" v-else>
							<image :src="v.img" />
						</navigator>
					</swiper-item>
				</block>
			</swiper>
		</view>
		<view class="subject">
			<view class="row" v-for="(v,k) in subject" @click="trunTo(v)">
				<image :src="v.img"></image>
				<view>{{v.name}}</view>
			</view>
		</view>
		<view class="info">
			<view class="option" v-if="vis">
				<view :class="type == 1?'left active':'left'" @click="cate(1)">
					<view class="row">
						<view class="title">资讯</view>
						<view>
							<text>政策解读</text>
						</view>
					</view>
				</view>
				<view class="left line"><text>|</text></view>
				<view :class="type == 2?'left active':'left'" @click="cate(2)">
					<view class="row">
						<view class="title">名师</view>
						<view>
							<text>难点精讲</text>
						</view>
					</view>
				</view>
				<view class="left line"><text>|</text></view>
				<view :class="type == 3?'left active':'left'" @click="cate(3)">
					<view class="row">
						<view class="title">课程</view>
						<view>
							<text>精选好课</text>
						</view>
					</view>
				</view>
				<view class="left line"><text>|</text></view>
				<view :class="type == 4?'left active':'left'" @click="cate(4)">
					<view class="row">
						<view class="title">推荐</view>
						<view>
							<text>优品分享</text>
						</view>
					</view>
				</view>
				<view class="clear"></view>
			</view>
			<view :class="'option '+optionTop" :style="'padding-top:'+all+'px'" v-if="vis">
				<view :class="type == 1?'left active':'left'" @click="cate(1)">
					<view class="row">
						<view class="title">资讯</view>
						<view>
							<text>政策解读</text>
						</view>
					</view>
				</view>
				<view class="left line"><text>|</text></view>
				<view :class="type == 2?'left active':'left'" @click="cate(2)">
					<view class="row">
						<view class="title">名师</view>
						<view>
							<text>难点精讲</text>
						</view>
					</view>
				</view>
				<view class="left line"><text>|</text></view>
				<view :class="type == 3?'left active':'left'" @click="cate(3)">
					<view class="row">
						<view class="title">课程</view>
						<view>
							<text>精选好课</text>
						</view>
					</view>
				</view>
				<view class="left line"><text>|</text></view>
				<view :class="type == 4?'left active':'left'" @click="cate(4)">
					<view class="row">
						<view class="title">推荐</view>
						<view>
							<text>优品分享</text>
						</view>
					</view>
				</view>
				<view class="clear"></view>
			</view>
			<view class="entry">
				<block v-for="(v,k) in information">
					<navigator class="row" hover-class="none" :url="v.url">
						<view class="left">
							<image :src="v.img"></image>
						</view>
						<view class="right">
							<view class="title"><text>{{v.name}}</text>{{v.title}}</view>
							<view class="bottom">
								<view class="left">{{v.time}}</view>
								<view class="right" v-if="v.origin">{{v.origin}}</view>
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
				indicatorDots: true,
				indicatorActiveColor: '#fafafa',
				indicatorColor: '#6edab4',
				subject: {},
				type: 1,
				optionTop: 'optionTop',
				height: 0,
				top: 0,
				all: 0,
				grade: '',
				gradeInfo: [],
				provice: '广东省',
				page:1,
				num:10,
				information:[],
				more: 'loading',
				contentText: {
					contentdown: "上拉显示更多",
					contentrefresh: "正在加载...",
					contentnomore: "没有更多数据了"
				},
				vis:''
			}
		},
		onLoad(e) {
			that = this
			//that.choseAddress()
			if(e.scene){
				uni.setStorage({
					key:'invite_id',
					data:e.scene
				})
			}
			if(e.invite_id){
				uni.setStorage({
					key:'invite_id',
					data:e.scene
				})
			}
		},
		onShareAppMessage() {

		},
		onPageScroll: function(e) {
			this.scroll = e.scrollTop
			const query = uni.createSelectorQuery().in(that);
			query.select('.option').boundingClientRect(data => {
				if(data){
					if(data.top > that.height+that.top){
						//隐藏
						that.optionTop = 'optionTop'
					}else{
						//显示
						that.optionTop = 'optionTops'
					}
				}
				
			}).exec();
			
		},
		methods: {
			trunTo(info) {
				uni.setStorage({
					key: 'subject',
					data: info,
					success() {
						uni.navigateTo({
							url: '/pages/semester/semester'
						})
					}
				})
			},
			//选择地址
			choseAddress() {
				that = this
				uni.getSetting({
				  success (res) {
					if(!res.authSetting['scope.userLocation']){
						uni.openSetting()
					}
					uni.chooseLocation({
						success: function(res) {
							let provice = ''
							if (res.address.match(/.+?(省)/g)) {
								provice = res.address.match(/.+?(省)/g)
							}
							if (!provice && res.address.match(/.+?(市)/g)) {
								provice = res.address.match(/.+?(市)/g)
							}
							if (!provice && res.address.match(/.+?(区)/g)) {
								provice = res.address.match(/.+?(区)/g)
							}
							that.provice = provice[0]
							uni.setStorage({
								key: 'provice',
								data: provice[0]
							})
						},
						fail(e) {
							uni.setStorage({
								key: 'is',
								data: 1
							})
						}
					});
					
				  }
				})
				
				
			},
			//切换选项
			cate: function(type) {
				this.type = type
				that.page = 1
				that.more = 'loading'
				that.information = []
				that.getInformation()
			},
			//选择年级
			onchange(e) {
				this.grade = e.detail.value[0]
				uni.setStorage({
					key: 'grade',
					data: e.detail.value[0]
				})
				that.getSubject()
				//更改用户年级信息
				if(app.globalData.api_token){
					uni.request({
						url:app.globalData.url+'user/grade',
						method:'post',
						data:{api_token:app.globalData.api_token,id:e.detail.value[0].value}
					})
				}
			},
			//banner信息
			getBanner() {
				uni.request({
					url: app.globalData.url + 'banner',
					success(res) {
						that.banner = res.data.data
					}
				})
			},
			//科目信息
			getSubject() {
				uni.request({
					url: app.globalData.url + 'cate/subject',
					data: {
						id: that.grade.value
					},
					success(res) {
						that.subject = res.data.data
						that.vis = res.data.vis
						if(!res.data.vis){
							uni.hideTabBar()
						}
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
								that.getSubject()
							},
							fail() {
								that.grade = res.data.data[0]
								uni.setStorage({
									key: 'grade',
									data: res.data.data[0]
								})
								that.getSubject()
							}
						})
					}
				})
			},
			//资讯
			getInformation(){
				let url = 'information/entry'
				if(that.type == 2 || that.type == 4){
					url = 'video/entry'
				}
				if(that.type == 3){
					url = 'course/entry'
				}
				uni.request({
					url: app.globalData.url + url,
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
		onReachBottom() {
			that.getInformation()
		},
		onShow() {
			let menuButtonInfo = uni.getMenuButtonBoundingClientRect()
			that.height = parseInt(menuButtonInfo.height)
			that.top = parseInt(menuButtonInfo.top)
			that.all = parseInt(menuButtonInfo.height) + parseInt(menuButtonInfo.top)
			that.getBanner()
			uni.setStorage({
				key: 'url',
				data: '/pages/index/index',
				success: function() {
					app.isLogin().then(function() {
						that.getGrade()
						that.getInformation()
					})
				}
			})
			uni.getStorage({
				key: 'provice',
				success(res) {
					that.provice = res.data
				},
				fail(e) {
					uni.getStorage({
						key: 'is',
						fail() {
							that.choseAddress()
						}
					})
					
				}
			})
		}
	}
</script>

<style>
	@import url('@/static/css/index.css')
</style>
