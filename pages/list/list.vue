<template>
    <list ref="list" :options="options" @success="onSuccess" @scroll="onScroll">
        <view class="item" v-for="(item, index) in list" :key="index">我是第{{index}}项</view>
		<view class="scroll-top" @click="$refs.list.scrollTop()" v-show="showToTop">回到顶部</view>
    </list>
</template>

<script>
  export default {
	  data() {
		  return {
			  list: [],
			  showToTop: false
		  }
	  },
      computed: {
          options() {
              return {
                  url: '/test', 
                  params: { 
				
                  }
              }

          }

      },
	  methods: {
		onSuccess(list) {
			this.list = list
			console.log("请求成功，当前的列表内容为：", list)
		},
		onScroll(e) {
			const {scrollTop} = e.target
			if(scrollTop > 100) {	
				this.showToTop = true
			} else {
				this.showToTop = false
			}
		}
	  },
      mounted() {
          this.$refs.list.request()
      }
  }
</script>
<style>
	.item {
		padding: 50upx;
		margin: 10upx;
		border-bottom: 1px solid #E5E5E5;
		text-align: center;
		font-size: 30upx;
	}
	.scroll-top {
		position: fixed;
		right: 0;
		bottom: 0;
		content: url(data:image/png;base64,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);
		width: 70upx;
		height: 70upx;
		padding: 20upx;
	}
</style>