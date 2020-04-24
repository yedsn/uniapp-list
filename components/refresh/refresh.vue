<template>
	<div class="container" @touchstart="refreshStart" @touchmove="refreshMove" @touchend="refreshEnd">
		<div class='refreshBox' :style="isTranform">
			<div class='refresh' :style="isZoom" :class="isEnd==2?'animationSmall':''">
				<div class='refreshWord' v-if="isEnd == 0">松开刷新</div>
				<div class='refreshCirle animation' v-if="isEnd == 1"></div>
				<image class='iconYes' v-if="isEnd==2"></image>
			</div>
		</div>
		<slot></slot>
	</div>
	</div>
</template>

<script>
	export default {
		name: 'refresh',
		props: {
			  isTop:{
				  type:Number,
				  default:1
			  }
		},
		data() {
			return {
				isTranf: 0,
				touchStart: '',
				touchMove: '',
				isEnd: 0
			};
		},
		
		methods: {
			refreshStart(e) {
				if (this.isEnd == 0 && this.isTop == 1) {
					this.touchStart = e.changedTouches[0].pageY
				}
			},
			refreshMove(e){
				if (this.isEnd == 0 && this.isTop == 1) {
					var touchStart = this.touchStart,
						oldMove = this.touchMove,
						newMove = e.changedTouches[0].pageY
					if (touchStart <= newMove) {
						var isTranf = touchStart > newMove ? 0 : newMove - touchStart
						this.isTranf = isTranf
						this.touchMove = e.changedTouches[0].pageY
					}
				} else{
					this.isTranf = 0
					this.isEnd = 0
					this.touchStart = 9999
				}
			},
			refreshEnd(e) {
				var that = this
				if (this.isEnd == 0 && this.isTop == 1) {
					if (this.isTranf >= 150) {
						this.isEnd = 1
						this.$emit('refresh');
						
					} else {
						this.isTranf = 0
					}
				}
			},
			endAfter(){
				this.isEnd = 2
				setTimeout(() => {
					this.isTranf = 0
					this.isEnd = 0
				}, 600)
			}
		},
		computed: {
			isTranform() {
				var isTranf = this.isTranf > 150 ? 150 : this.isTranf
				var isTemp = `transform: translateY(${isTranf-100}px);`
				return isTemp
			},
			isZoom() {
				var isTranf = this.isTranf > 125 ? 125 : this.isTranf
				var isTemp = `zoom:${isTranf/125};`
				return isTemp
			}
		}

	}
</script>

<style lang="css" scoped>
	.container {
		min-height: 100vh;
	}
	.refreshBox {
		margin:0 auto;
		width: 100%;
		height: 100upx;
		overflow: hidden;
		display: flex;
		align-items: center;
		justify-content: center;
		max-height: 300upx;
		position: fixed;
		z-index: 99999;
		top: 0;
		left: 0;
		transform: translateY(-100upx);
	}

	.animationSmall {
		animation: small 1.1s both;
	}

	@keyframes small {
		0% {
			transform: scale(1)
		}
		20%{
			transform: scale(1.4)
		}
		100% {
			transform: scale(0)
		}
	}
	.refreshWord{
		width: 150upx;
		height: 26upx;
		font-size: 24upx;
		line-height: 26upx;
		text-align: center;
		border-radius: 26upx;
	}

	.refresh {
		min-width: 50upx;
		display: flex;
		align-items: center;
		justify-content: center;
		height: 50upx;
		background: #FFFFFF;
		box-shadow: 0 0 16upx 0 rgba(0, 0, 0, 0.10);
		border-radius: 50upx;
	}

	.refreshCirle {
		width: 26upx;
		height: 26upx;
		border-radius: 50%;
		display: inline-block;
		position: relative;
		border: 6upx solid black;
		border-bottom-color: transparent;
		border-top-color: transparent;
	}

	.animation {
		animation: rotate 1.1s infinite;
		animation-timing-function: cubic-bezier(0.3, 1.65, 0.7, -0.65);
	}

	@keyframes rotate {
		0% {
			transform: rotate(0deg);
		}

		100% {
			transform: rotate(360deg);
		}
	}

	.true {
		color: black;
	}

	.iconYes {
		width: 34upx;
		height: 34upx;
		content: url('data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAKQAAACkCAYAAAAZtYVBAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyJpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNiAoV2luZG93cykiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6M0I4NEI3Qjk0RUI4MTFFOTg2MTdFRUQ2OTQ1RjM5NzYiIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6M0I4NEI3QkE0RUI4MTFFOTg2MTdFRUQ2OTQ1RjM5NzYiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0ieG1wLmlpZDozQjg0QjdCNzRFQjgxMUU5ODYxN0VFRDY5NDVGMzk3NiIgc3RSZWY6ZG9jdW1lbnRJRD0ieG1wLmRpZDozQjg0QjdCODRFQjgxMUU5ODYxN0VFRDY5NDVGMzk3NiIvPiA8L3JkZjpEZXNjcmlwdGlvbj4gPC9yZGY6UkRGPiA8L3g6eG1wbWV0YT4gPD94cGFja2V0IGVuZD0iciI/PhO5GWwAAAhXSURBVHja7JwNdE9l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');
	}
</style>
