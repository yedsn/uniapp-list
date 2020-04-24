<template>
	<view>
		<refresh ref="refresh" @refresh='$refs.list.refresh()'>
			<list ref="list" :options="options" @success="$refs.refresh.endAfter()">
				<template v-slot="{list}">
					<view class="title">列表组件封装</view>
					<!-- 循环数据 -->
					<view class="item" v-for="(item, index) in list" :key="index">我是第{{index}}项</view>
				</template>
			</list>
		</refresh>
	</view>
</template>

<script>
	export default {
		computed: {
			
			// 定义请求的配置，配置详情请查看文档说明
			options() {
				return {
					url: '/test',
					params: {
						
					}
				}
				
			}
			
		},
		mounted() {
			// 调用组件的request方法开始获取数据
			// 为什么不默认就请求？因为很多时候组件并不知道你的options是不是已经配置好了，所以还是手动请求比较靠谱点
			this.$refs.list.request()
		}
	}
</script>

<style>
	.title {
		padding: 20upx;
		background-color: #E5E5E5;
		text-align: center;
	}
	.item {
		padding: 50upx;
		margin: 10upx;
		border-bottom: 1px solid #E5E5E5;
		text-align: center;
		font-size: 30upx;
	}
</style>
