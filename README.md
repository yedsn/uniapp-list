### 列表组件封装最佳实践

#### 效果展示
![效果展示](https://img01.sogoucdn.com/app/a/100520146/A195743EFF3736266871B6F32014EBF2)

*目前只在小程序和H5页面测试过没问题，其他的需要自己测试*

`此组件经过封装后最大的好处在于，页面只需要极少必须的代码，就可以完成列表的渲染，参考pages/index/index`

****

#### list组件

封装的渲染列表组件

- 使用说明

    - 第一步：配置list组件
        配置list组件中的getData方法配置接口获取数据的方式

    - 第二步：在页面中使用组件

      ```html
      <template>
          <list ref="list" :options="options">
              <template v-slot="{list}">
                  <view class="item" v-for="(item, index) in list" :key="index">我是第{{index}}项</view>
              </template>
          </list>
      </template>

      <script>
        export default {
            computed: {
                options() {
                    return {
                        url: '/test', 
                        params: { 
				
                        }
                    }

                }

            },
            mounted() {
                this.$refs.list.request()
            }
        }
      </script>
      ```

- 配置说明

  - options 配置对象

    | 项        | 说明                                                         | 默认值 |
    | --------- | ------------------------------------------------------------ | ------ |
    | url       | 请求的地址，在组件的getData方法中会拿到                      |        |
    | params    | 请求携带的参数，在组件的getData方法中会拿到                  | {}     |
    | converter | 数据转换器，在组件的getData方法中会拿到，需要特殊处理数据时可以用到 |        |

  - noMoreTip 没有更多提示，默认是“暂无更多”

  - loadingTip 正在加载提示，模式是“加载中...”

- 事件说明

  - success 请求成功事件，在组件请求成功后会触发，参数为当前列表内容
  - scroll 滚动事件，列表滚动时会触发，参数为滚动事件对象e

- 组件可调用方法说明

  - refresh 刷新组件数据，页数回到第一页
  - scrollTop 滚动列表到顶部



#### refresh组件 

封装的下拉刷新组件，可包裹在元素外层，使得在元素下拉时促发下拉刷新效果。

- 使用说明

  ```
  <template>
  	<refresh ref="refresh" @refresh='onRefresh'>
  		<view>
  			我是可以刷新的页面
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
  ```

- 事件说明

  - refresh 用户下拉后触发此事件

- 组件可调用方法说明

  - endAfter 通知下拉刷新组件刷新已经完成

****
本组件 基于`zwyboom`的uni-list组件进行修改 https://github.com/seventhcode/uni-list，感谢 `@zwyboom` 的分享。

****
uniapp插件市场地址：https://ext.dcloud.net.cn/plugin?id=1685
