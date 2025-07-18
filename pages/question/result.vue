<template>
	<view>
		<view class="top">
			<view class="bg">
				<view class="content">
					<view class="result">
						<view class="left">
							<view class="title">{{desc.title}}</view>
							<view class="text">正确率：{{desc.persent}}%</view>
							<view class="text">{{desc.text}}</view>
						</view>
						<view class="right">
							<view style="display: flex;justify-content: center;position: relative;">
								<cmd-progress type="circle" :percent="desc.persent" stroke-color="#4bb1fa" :stroke-width="8"
									:width="90" :showInfo="false"></cmd-progress>
								<view style="position: absolute;text-align: center;top: 30%;">
									<view style="font-size:20rpx; color:#999999"><text
											style="font-size: 36rpx; color:#0f93ff">{{desc.currect}}</text>/{{desc.num}}</view>
									<view style="font-size:20rpx; color:#999999">答对数/总题数</view>
								</view>
							</view>
						</view>
						<view class="clear"></view>
					</view>
					<view class="part"></view>
					<view class="error">
						<view class="title">
							<image src="../../static/image/icon_error.png"></image>
							<text>错题本</text>
						</view>
						<view class="entry">
							<block v-for="(v,k) in info">
								<view class="row" v-if="v.check == -1">
									<view class="name">{{k+1}} . {{v.title}}</view>
									<rich-text :nodes="'考点：'+v.point"></rich-text>
								</view>
							</block>
						</view>
					</view>
					<view class="question_result">
						<view class="top_row">
							<text>题目解析汇总</text>
							<view class="right">{{desc.subTitle}}</view>
						</view>
						<view class="question_result_content">
							<block v-for="(v,k) in info">
								<view class="q_row">
									<view class="title">
										{{k+1}} . {{v.title}}
									</view>
									<view class="option" v-if="v.check == 1">
										<view :class="v.check_A?'row correct':'row'"><text>A</text>{{v.option_A}}</view>
										<view :class="v.check_B?'row correct':'row'"><text>B</text>{{v.option_B}}</view>
										<view :class="v.check_C?'row correct':'row'"><text>C</text>{{v.option_C}}</view>
										<view :class="v.check_D?'row correct':'row'"><text>D</text>{{v.option_D}}</view>
									</view>
									<view class="option" v-if="v.check == -1">
										<view :class="v.check_A?'row error':'row'"><text>A</text>{{v.option_A}}</view>
										<view :class="v.check_B?'row error':'row'"><text>B</text>{{v.option_B}}</view>
										<view :class="v.check_C?'row error':'row'"><text>C</text>{{v.option_C}}</view>
										<view :class="v.check_D?'row error':'row'"><text>D</text>{{v.option_D}}</view>
									</view>
									<view class="analysis">
										<rich-text :nodes="v.explain"></rich-text>
										<view class="content">故选<text>{{v.answers}}</text></view>
									</view>
								</view>
							</block>
						</view>
					</view>
				</view>
			</view>
			
		</view>
		<view class="bg2"></view>
	</view>
</template>

<script>
	import cmdProgress from "@/components/cmd-progress/cmd-progress.vue"
	const app = getApp()
	var that
	export default {
		components: {
			cmdProgress
		},
		data() {
			return {
				id:'',
				desc:'',
				info:''
			}
		},
		onLoad(e) {
			that = this
			that.id = e.id
		},
		onShow() {
			uni.setStorage({
				key: 'url',
				data: '/pages/question/result?id='+that.id,
				success: function() {
					app.isLogin().then(function() {
						that.getInfo()
					})
				}
			})

		},
		methods: {
			getInfo(){
				uni.request({
					url:app.globalData.url+'user_answer/result',
					data:{api_token:app.globalData.api_token,id:that.id},
					success(res) {
						if(res.data.result == 1){
							that.desc = res.data.title
							that.info = res.data.data
						}
					}
				})
			}
		}
	}
</script>

<style>
	@import url('@/static/css/question_result.css')
</style>
