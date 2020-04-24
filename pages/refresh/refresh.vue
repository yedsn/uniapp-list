<template>
	<refresh ref="refresh" @refresh='onRefresh'>
		<view>
			我是可以下拉刷新的页面
		</view>
	</refresh>
</template>

<script>
	export default {
		methods: {
			onRefresh() {
				uni.showToast({
					icon: 'none',
					title: `开始刷新`
				})
				setTimeout(() => {
					uni.showToast({
						icon: 'none',
						title: `完成`
					})
					this.$refs.refresh.endAfter() //通知下拉刷新组件刷新已经完成
				}, 1000)
			}
		}
	}
</script>
