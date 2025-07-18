<template>
	<view>
		<video :src="url"></video>
	</view>
</template>

<script>
	var that
	export default {
		data() {
			return {
				//1视频 2文稿 3笔记
				url:''
			}
		},
		methods: {
			
		},
		onLoad(e) {
			that = this
			that.url = e.url
		}
	}
</script>

<style>
video{
	width: 100%;
}
</style>
