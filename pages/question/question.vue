<template>
	<view>
		<view class="top" id="top">
			<view class="title">{{title}}</view>
			<view class="speed">
				<view class="left">{{subTitle}}</view>
				<view class="right">
					{{current+1}}<text>/{{num}}</text>
					<view class="persent">
						<text :style="'width:'+persent"></text>
					</view>
				</view>
			</view>
		</view>
		<swiper class="swiper" :autoplay="autoplay" :interval="interval" :duration="duration" :circular="circular"
			:current="current" :style="'height:'+height+'px'" @change="changeType">
			<block v-for="(v,k) in info">
				<swiper-item style="overflow: auto;">
					<view class="content swiper-item">
						<view class="title">
							{{k+1}}.({{v.type == 1?'单选题':'多选题'}}){{v.title}}
						</view>
						<view class="option">
							<view :class="v.check_A?'active row':'row'" @click="chose(k,1)">A<text>|</text>{{v.option_A}}</view>
							<view :class="v.check_B?'active row':'row'" @click="chose(k,2)">B<text>|</text>{{v.option_B}}</view>
							<view :class="v.check_C?'active row':'row'" @click="chose(k,3)">C<text>|</text>{{v.option_C}}</view>
							<view :class="v.check_D?'active row':'row'" @click="chose(k,4)">D<text>|</text>{{v.option_D}}</view>
						</view>
						<!--<view class="analysis">
							<rich-text :nodes="v.explain"></rich-text>
						</view>-->
					</view>
				</swiper-item>
			</block>
		</swiper>
		<view class="nav">
			<view class="back" @click="back()">返回</view>
			<view class="next" @click="next()"  v-if="current+1 != num">下一题</view>
			<view class="next" v-if="current+1 == num" @click="submit">提交</view>
		</view>
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
				persent: '20%',
				num:5,
				info:[],
				id:'',
				title:'',
				subTitle:''
			}
		},
		onLoad(e) {
			that = this
			that.id = e.id
		},
		onShow() {
			uni.setStorage({
				key: 'url',
				data: '/pages/question/question?id='+that.id,
				success: function() {
					app.isLogin().then(function() {
						app.checkLogin(1)
						that.getInfo()
					})
				}
			})
			const query = uni.createSelectorQuery().in(this);
			query.select('#top').boundingClientRect(data => {
				uni.getSystemInfo({
					success: res => {
						//导航高度
						that.height = parseInt(res.windowHeight) - parseInt(data.height)
					}
				})
			}).exec();
		},
		methods: {
			//选题
			chose(k,num){
				if(!app.globalData.api_token){
					app.checkLogin()
					return;
				}
				for(let i in that.info){
					if(i == k){
						if(that.info[i].type == 1){
							that.info[i].check_A = ''
							that.info[i].check_B = ''
							that.info[i].check_C = ''
							that.info[i].check_D = ''
						}
						switch(parseInt(num)){
							case 1:

								if(that.info[i].check_A){
									that.info[i].check_A = ''
								}else{
									that.info[i].check_A = 1
								}
							break;
							case 2:
								if(that.info[i].check_B){
									that.info[i].check_B = ''
								}else{
									that.info[i].check_B = 1
								}
							break;
							case 3:
								if(that.info[i].check_C){
									that.info[i].check_C = ''
								}else{
									that.info[i].check_C = 1
								}
							break;
							case 4:
								if(that.info[i].check_D){
									that.info[i].check_D = ''
								}else{
									that.info[i].check_D = 1
								}
							break;
						}
					}
				}
			},
			changeType(e){
				that.current = e.detail.current
				that.persent = (parseInt(e.detail.current)+1)/parseInt(that.num)*100+'%'
			},
			next(){
				if(that.current+1 < that.num){
					that.current++
				}
			},
			back(){
				if(that.current != 0){
					that.current--
				}
			},
			getInfo(){
				uni.request({
					url:app.globalData.url+'question_bank/entry',
					data:{id:that.id,api_token:app.globalData.api_token},
					success(res) {
						that.info = res.data.data
						that.num = res.data.data.length
						that.persent = 1/parseInt(res.data.data.length)*100+'%'
						that.title = res.data.title.title
						that.subTitle = res.data.title.subTitle
						uni.setNavigationBarTitle({
							title: res.data.title.chapters_name
						})
					}
				})
			},
			submit(){
				if(!app.globalData.api_token){
					app.checkLogin()
					return;
				}
				let is = 1;
				for(let i in that.info){
					if(!that.info[i].check_A && !that.info[i].check_B && !that.info[i].check_C && !that.info[i].check_D){
						is = ''
					}
				}
				if(!is){
					uni.showToast({
						title:'请完成所有题目再提交',
						icon:'none'
					})
					return;
				}
				uni.showModal({
					title:'确认提交?',
					success(res) {
						if (res.confirm) {
							uni.showLoading()
							let data = that.info
							let res = []
							for(let i in data){
								let answer = []
								if(data[i].check_A){
									answer.push('1')
								}
								if(data[i].check_B){
									answer.push('2')
								}
								if(data[i].check_C){
									answer.push('3')
								}
								if(data[i].check_D){
									answer.push('4')
								}
								res.push({id:data[i].id,chose:answer.join(',')})
							}
							uni.request({
								url:app.globalData.url+'user_answer/add',
								method:'PUT',
								data:{api_token:app.globalData.api_token,data:res,id:that.id},
								success(res) {
									if(res.data.result == 1){
										setTimeout(function(){
											uni.redirectTo({
												url:'/pages/question/result?id='+that.id
											})
										},1000)
										
									}
									if(res.data.result == -3){
										setTimeout(function(){
											uni.redirectTo({
												url:'/pages/question/result?id='+that.id
											})
										},1000)
									}
									uni.showToast({
										title:res.data.msg,
										icon:'none'
									})
								}
							})
						}
					}
				})
			},
		}
	}
</script>

<style>
	@import url('@/static/css/question.css')
</style>
