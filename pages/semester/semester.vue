<template>
	<view>
		<view class="option">
			<block v-for="(v,k) in semester">
				<view :class="type == k ? 'active':''" @click="changeTypes(k)">
					<text>{{v.name}}</text>
					<image src="../../static/image/line.png"></image>
				</view>
			</block>
		</view>
		<swiper class="swiper" :autoplay="autoplay" :interval="interval" :duration="duration" :circular="circular"
			:current="current" :style="'height:'+height+'px'" @change="changeType">
			<block v-for="(v,k) in semester">
				<swiper-item style="overflow: auto;">
					<view class="content swiper-item">
						<block v-for="(val,key) in v.lesson">
							<view :class="val.active?'row active':'row'">
								<view class="title" @click="vis(k,key,val.active)" v-if="val.chapter.length">
									<view class="left">
										{{val.name}}<text>{{val.desc}}</text>
									</view>
									<view class="right">
										<image src="../../static/image/show_all.png"></image>
									</view>
									<view class="clear"></view>
								</view>
								<view class="entry">
									<view class="sub" v-if="val.content && val.content.video">
										<view class="left">
											<view class="part">
												<view class="part1">{{val.content.video_title}}</view>
											</view>
										</view>
										<view class="right" v-if="api_token && val.content.is_video" @click="open(1,val.content.video,val.content.id)"><text>查看</text></view>
										<view class="right" v-if="api_token && !val.content.is_video"><text>积分不足</text></view>
										<navigator class="right" hover-class="none" v-if="!api_token" url="/pages/login/login"><text>登陆后才能查看</text></navigator>
										<view class="clear"></view>
									</view>
									<view class="sub" v-if="val.content && val.content.file">
										<view class="left">
											<view class="part">
												<view class="part1">{{val.content.file_title}}</view>
											</view>
										</view>
										<view class="right" v-if="api_token && val.content.is_file" @click="open(2,val.content.file,val.content.id)"><text>查看</text></view>
										<view class="right" v-if="api_token && !val.content.is_file"><text>积分不足</text></view>
										<navigator class="right" hover-class="none" v-if="!api_token" url="/pages/login/login"><text>登陆后才能查看</text></navigator>
										<view class="clear"></view>
									</view>
									<block v-for="(vol,koy) in val.chapter">
										<view class="sub">
											<view class="left">
												<view class="part">
													<view class="part1">{{vol.name}}</view>
													<view class="part2" v-if="vol.is">平均正确率{{vol.persent}}</view>
												</view>
											</view>
											<navigator class="right" hover-class="none" :url="'/pages/question/question?id='+vol.id" v-if="!vol.is && vol.can"><text>去看题</text></navigator>
											<navigator class="right" hover-class="none" :url="'/pages/question/result?id='+vol.id" v-if="vol.is && vol.can"><text>去看题</text></navigator>
											<navigator class="right" hover-class="none" v-if="!vol.can && app.globalData.api_token"><text>积分不足</text></navigator>
											<navigator class="right" hover-class="none" v-if="!api_token" url="/pages/login/login"><text>登陆后才能查看</text></navigator>
											<view class="clear"></view>
										</view>
									</block>
									<view class="sub" v-if="val.content && val.content.note">
										<view class="left">
											<view class="part">
												<view class="part1">{{val.content.note_title}}</view>
											</view>
										</view>
										<view class="right" v-if="api_token && val.content.is_note" @click="open(3,val.content.note,val.content.id)"><text>查看</text></view>
										<view class="right" v-if="api_token && !val.content.is_note"><text>积分不足</text></view>
										<navigator class="right" hover-class="none" v-if="!api_token" url="/pages/login/login"><text>登陆后才能查看</text></navigator>
										<view class="clear"></view>
									</view>
								</view>
							</view>
						</block>
					</view>
				</swiper-item>
			</block>
		</swiper>
	</view>
</template>

<script>
	const app = getApp()
	var that
	export default {
		data() {
			return {
				autoplay: false,
				interval: 2000,
				duration: 500,
				circular: false,
				current: 0,
				height: 0,
				heightt: 0,
				type: 0,
				grades_id: '',
				subjects_id: '',
				semester: [],
				api_token:''
			}
		},
		onLoad(e) {
			that = this
		},
		onShow() {
			const query = uni.createSelectorQuery().in(this);
			query.select('.option').boundingClientRect(data => {
				uni.getSystemInfo({
					success: res => {
						//导航高度
						that.height = parseInt(res.windowHeight) - parseInt(data.height)
					}
				})
			}).exec();
			uni.setStorage({
				key: 'url',
				data: '/pages/semester/semester',
				success: function() {
					app.isLogin().then(function() {
						//app.checkLogin(1)
						that.getSemester()
						that.api_token = app.globalData.api_token
					})
				}
			})
			
		},
		methods: {
			open(type,file,id){
				uni.request({
					url:app.globalData.url+'user/score/used',
					method:'post',
					data:{id:id,type:type,api_token:that.api_token},
					success(res) {
						if(res.data.result == -1){
							uni.showToast({
								title:res.data.msg,
								icon:none
							})
							return;
						}
						if(res.data.result == 1){
							if(type == 1){
								uni.navigateTo({
									url:'/pages/lesson/content?url='+file
								})
								return;
							}
							uni.showLoading({
								title:'文件加载中',
							})
							wx.downloadFile({
							  url: file,
							  success: function (res) {
							    const filePath = res.tempFilePath
							    wx.openDocument({
							      filePath: filePath,
							      success: function (res) {
									 uni.hideLoading()
							      }
							    })
							  }
							})
						}
					}
				})
				
			},
			vis(k,key,is){
				for(let i in that.semester){
					if(i == k){
						for(let e in that.semester[i].lesson){
							if(e == key){
								if(is == 1){
									that.semester[i].lesson[e].active = 0
								}else{
									that.semester[i].lesson[e].active = 1
								}
							}
						}
					}
				}
			},
			changeType(e) {
				that.type = e.detail.current
			},
			changeTypes(type) {
				that.type = type
				that.current = type
			},
			getTitle() {

			},
			getSemester() {
				uni.getStorage({
					key: 'grade',
					success(res) {
						uni.getStorage({
							key: 'subject',
							success(e) {
								uni.setNavigationBarTitle({
									title: res.data.text + e.data.name
								})
								uni.request({
									url: app.globalData.url + 'cate/semester',
									data: {
										subjects_id: e.data.id,
										grades_id: res.data.value,
										api_token:app.globalData.api_token
									},
									success(res) {
										that.semester = res.data.data
									}
								})
							}
						})
					}
				})

			}
		}
	}
</script>

<style>
	@import url('@/static/css/semester.css')
</style>
