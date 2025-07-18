<template>
	<view>
		<rich-text :nodes="info"></rich-text>
	</view>
</template>

<script>
	const app = getApp()
	var that
	export default {
		data() {
			return {
				info:''
			}
		},
		methods: {
			
		},
		onLoad() {
			that = this
			uni.request({
				url:app.globalData.url+'customer_service',
				success(res) {
					that.info = res.data.data
				}
			})
		}
	}
</script>

<style>

</style>
