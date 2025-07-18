<template>
	<view>
		<view class="top">
			<view class="row" :style="'height:'+height+'px; padding-top:'+top+'px; line-height:'+height+'px;'">
				<navigator hover-class="none" url="/pages/user_info/user_info" class="left">设置</navigator>
				<view class="right">消息</view>
			</view>
			<view class="content">
				<view class="left img">
					<image :src="userInfo.profile_pic" mode="aspectFit"></image>
				</view>
				<view class="left">
					<view class="part">
						<view>
							<text>{{userInfo.name}}</text>
							<image src="../../static/image/star.png"></image>
							<image src="../../static/image/star.png"></image>
							<image src="../../static/image/star.png"></image>
							<image src="../../static/image/star.png"></image>
							<image src="../../static/image/star.png"></image>
						</view>
						<view>我的积分 : {{userInfo.score}}</view>
					</view>
				</view>
				<navigator class="right" hover-class="none" url="/pages/achievement/achievement">
					<text>成就列表</text>
				</navigator>
			</view>
		</view>
		<view class="bg2"></view>
		<view class="fun">
			<navigator class="row" hover-class="none" url="/pages/score/score">
				<view class="img">
					<image src="../../static/image/row1.png" mode="aspectFit"></image>
				</view>
				<view class="text">积分</view>
			</navigator>
			<navigator class="row" hover-class="none" url="/pages/achievement/achievement">
				<view class="img">
					<image src="../../static/image/row2.png" mode="aspectFit"></image>
				</view>
				<view class="text">成就</view>
			</navigator>
			<button open-type="share">
				<view class="img">
					<image src="../../static/image/row3.png" mode="aspectFit"></image>
				</view>
				<view class="text">分享</view>
			</button>
			<navigator class="row" hover-class="none" url="/pages/upload/upload">
				<view class="img">
					<image src="../../static/image/row4.png" mode="aspectFit"></image>
				</view>
				<view class="text">上传</view>
			</navigator>
		</view>
		<view class="sign">
			<view class="row">
				<text>连续签到{{num}}天</text>
				<view class="right">
					<text v-if="!userInfo.is" @click="sign()">签到</text>
					<text class="active" v-else>已签到</text>
				</view>
			</view>
			<view class="detail">
				<block v-for="(v,k) in SignDetail">
					<view :class="v.is?'part active':'part'">
						<view v-if="v.today == -1">{{v.name}}</view>
						<view v-else>今天</view>
						<view>
							<image src="../../static/image/signs.png" mode="aspectFit" v-if="v.is == 1"></image>
							<image src="../../static/image/sign1.png" mode="aspectFit" v-else></image>
						</view>
						<view class="score">+{{v.score}}</view>
					</view>
				</block>
			</view>
		</view>
		<view class="often">
			<view class="title">常用功能</view>
			<button open-type="share" class="row btn">
				<view class="left"><image src="../../static/image/often1.png" mode="aspectFit"></image></view>
				<view class="right">
					<text>专项推荐码</text>
					<view class="right"><uni-icons type="arrowright" size="20" color="#cbcbcb"></uni-icons></view>
				</view>
			</button>
			<navigator class="row" hover-class="none" url="/pages/customer_service/customer_service">
				<view class="left"><image src="../../static/image/often2.png" mode="aspectFit"></image></view>
				<view class="right">
					<text>官方客服</text>
					<view class="right"><uni-icons type="arrowright" size="20" color="#cbcbcb"></uni-icons></view>
				</view>
			</navigator>
			<view class="row" @click="feedBack()">
				<view class="left"><image src="../../static/image/often3.png" mode="aspectFit"></image></view>
				<view class="right">
					<text>意见反馈</text>
					<view class="right"><uni-icons type="arrowright" size="20" color="#cbcbcb"></uni-icons></view>
				</view>
			</view>
			<navigator class="row" hover-class="none" url="/pages/user_info/user_info">
				<view class="left"><image src="../../static/image/often4.png" mode="aspectFit"></image></view>
				<view class="right">
					<text>资料设置</text>
					<view class="right"><uni-icons type="arrowright" size="20" color="#cbcbcb"></uni-icons></view>
				</view>
			</navigator>
			<navigator class="row" hover-class="none" url="/pages/introduction/introduction">
				<view class="left"><image src="../../static/image/often4.png" mode="aspectFit"></image></view>
				<view class="right">
					<text>小猿预习</text>
					<view class="right"><uni-icons type="arrowright" size="20" color="#cbcbcb"></uni-icons></view>
				</view>
			</navigator>
		</view>
		<e-modal :visible.sync="vis" @cancel="handleCancel">
			<view class="model">
				<view class="group">
					<view class="left text">*反馈内容</view>
					<view class="left">
						<textarea @input="changevExt" :value="content"></textarea>
					</view>
					<view class="clear"></view>
				</view>
				<view class="submit">
					<button size="mini" @click="submit()">提交</button>
				</view>
			</view>
		</e-modal>
	</view>
</template>

<script>
	const app = getApp()
	var that
	export default {
		data() {
			return {
				height: 0,
				top: 0,
				SignDetail:[],
				userInfo:'',
				num:0,
				sharePic:'',
				vis:false,
				content:''
			}
		},
		onShow() {
			let menuButtonInfo = uni.getMenuButtonBoundingClientRect()
			that.height = parseInt(menuButtonInfo.height)
			that.top = parseInt(menuButtonInfo.top)
			uni.setStorage({
				key: 'url',
				data: '/pages/mine/mine',
				success: function() {
					app.isLogin().then(function() {
						app.checkLogin(1)
						that.getSignDetail()
						that.getUserInfo()
						that.getSharePic()
					})
				}
			})
		},
		onLoad() {
			that = this
		},
		onShareAppMessage() {
			return {
			  title: that.userInfo.name,
			  path: '/pages/index/index?invite_id='+that.userInfo.id,
			  imageUrl:that.sharePic,
			}
		},
		onShareTimeline(){
			return {
			  title: that.userInfo.name,
			  path: '/pages/index/index?invite_id='+that.userInfo.id,
			  
			}
		},
		methods: {
			changevExt(e){
				that.content = e.detail.value
			},
			submit(){
				if(!that.content){
					uni.showToast({
						title:'请填写反馈意见',
						icon:'none'
					})
					return;
				}
				uni.request({
					url:app.globalData.url+'feed_back',
					method:'post',
					data:{api_token:app.globalData.api_token,content:that.content},
					success(res) {
						uni.showToast({
							title:res.data.msg,
							icon:'none'
						})
						that.vis = false
						that.content = ''
					}
				})
			},
			feedBack(){
				that.vis = true
			},
			handleCancel(){
				that.vis = false
			},
			getSignDetail(){
				uni.request({
					url:app.globalData.url+'sign/detail',
					data:{api_token:app.globalData.api_token},
					success(e) {
						if(e.data.result == 1){
							that.SignDetail = e.data.data
							that.num = e.data.num
						}
					}
				})
			},
			getUserInfo(){
				uni.request({
					url:app.globalData.url+'user/info',
					data:{api_token:app.globalData.api_token},
					success(e) {
						if(e.data.result == 1){
							that.userInfo = e.data.data
						}
					}
				})
			},
			sign(){
				uni.showLoading()
				uni.request({
					url:app.globalData.url+'sign/add',
					data:{api_token:app.globalData.api_token},
					method:'POST',
					success(e) {
						uni.showToast({
							title:e.data.msg
						})
						if(e.data.result == 1){
							that.getSignDetail()
							that.getUserInfo()
						}
					},
					complete() {
						uni.hideLoading()
					}
				})
			},
			getSharePic(){
				uni.request({
					url:app.globalData.url+'share/code',
					data:{api_token:app.globalData.api_token},
					method:'GET',
					success(e) {
						that.sharePic = e.data.data
					}
				})
			}
		}
	}
</script>

<style>
	@import url('@/static/css/mine.css')
</style>
