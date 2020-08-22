<template>
	<!-- @touchmove.stop.prevent是为了阻止手机默认的滑动事件 -->
	<view class="example" @touchmove.stop.prevent>
		<view class="header">
			<view class="title">
				<view @tap="changeShowType(1)" :class="showType==1?'activeContent':''">
					<view :class="showType==1?'activeTitle':''" class="words fontHKFY">tab1</view>
					<view :class="showType==1?'activeHr':''" class="hr"></view>
				</view>
				<view @tap="changeShowType(2)" :class="showType==2?'activeContent':''">
					<view :class="showType==2?'activeTitle':''" class="words fontHKFY">tab2</view>
					<view :class="showType==2?'activeHr':''" class="hr"></view>
				</view>
				<view @tap="changeShowType(3)" :class="showType==3?'activeContent':''">
					<view :class="showType==3?'activeTitle':''" class="words fontHKFY">tab3</view>
					<view :class="showType==3?'activeHr':''" class="hr"></view>
				</view>
			</view>
		</view>
		<!-- 多个tab切换的时候，注意插件的isTab属性， 它可以用来防止两个tab之间用手滑动时造成的误操作，但是会降低下拉刷新的灵敏度-->
		<swiper :current='showType-1' @change="onChange">
			<swiper-item>
				<tab1 ref='tab1'></tab1>
			</swiper-item>
			<swiper-item>
				<tab2 ref='tab2'></tab2>
			</swiper-item>
			<swiper-item>
				<tab3 ref='tab3'></tab3>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
	import tab1 from "../../components/tab/tab1.vue"
	import tab2 from "../../components/tab/tab2.vue"
	import tab3 from "../../components/tab/tab3.vue"
	export default{
		data(){
			return{
				showType:1,
			}
		},
		components:{
			tab1,
			tab2,
			tab3
		},
		onLoad(){
			
		},
		methods:{
			/* 切换显示内容 */
			changeShowType(type){
				this.showType = type;
			},
			/* 手动滑动swiper触发的事件 */
			onChange(e){
				this.showType = e.detail.current+1;
			},
		},
	}
</script>

<style lang="scss" scoped>
	.example{
		width: 100%;
		.header{
			width: 100%;
			height: 116rpx;
			background: #FFFFFF;
			.title{
				width: 100%;
				padding: 40rpx 30rpx 0rpx;
				display: flex;
				justify-content: space-around;
				align-items: center;
				background-color: rgb(176, 176, 176);
				>view{
					flex: 1;
					display: flex;
					flex-direction: column;
					justify-content: center;
					align-items: center;
					position: relative;
					padding: 16rpx 0;
					border-radius: 16rpx 16rpx 0 0;
					.words{
						font-size: 28rpx;
						color: #FFFFFF;
					}
					.hr{
						width: 50rpx;
						margin-top: 8rpx;
						height:5rpx;
						background:transparent;
						border-radius:10px;
					}
					.activeTitle{
						color: #333333;
					}
					.activeHr{
						background:#333333;
					}
				}
				.activeContent{
					background: #FFFFFF;
				}
			}
		}
		swiper{
			width: 100%;
			/* #ifdef H5*/
			height: calc(100vh - 116rpx - 44px);
			/* #endif */
			
			/* #ifndef H5*/
			height: calc(100vh - 116rpx);
			/* #endif */
			background: #FFFFFF;
		}
		swiper-item{
			width: 100%;
			height: 100%;
		}
	}
</style>
