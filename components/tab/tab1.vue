<template>
	<view class="example">
			<hr-pull-load
			 @refresh='refresh' 
			 @loadMore='loadMore' 
			 :height='-1' 
			 :pullHeight='50' 
			 :maxHeight='100' 
			 :lowerThreshold='20'
			 :bottomTips='bottomTips' 
			 :isAllowPull="true"
			 :isTab='true'
			 ref='hrPullLoad'>
			 <!-- 插入自己的数据-->
				<view class="list" v-for="(item,index) in exampleInfo" :key='index'>
					<view class="left">
						<text class="num">99</text>
						<text class="time">00</text>
					</view>
					<view class="middle">
						<text class="nickName">
							派大星和海绵宝宝
						</text>
					</view>
					<view class="right">
						<text class="dName">1221</text>
						<text class="rank">0127</text>
					</view>
				</view>
			</hr-pull-load> 
	</view>
</template>

<script>
	import hrPullLoad from '@/components/hr-pull-load/hr-pull-load.vue'; 
	export default{
		data(){
			return{
				exampleInfo:[],
				bottomTips:'',
				currentPage:1,
			}
		},
		components:{
			hrPullLoad
		},
		created(){
			
			this.getExampleData(1);
		},
		methods:{
			/* 调用接口从后台获取数据，这里的逻辑处理大家可以参考，具体的处理大家可以自定义，需要注意的是：
			1.bottomTips用来控制触发加载更多时的提示
			2.this.$refs.hrPullLoad.reSet()用来重置下拉刷新状态
			*/
			getExampleData(type){
				/* type 1代表下拉刷新 2代表加载更多 */
				setTimeout(()=>{
					/* 接口获取到的数据 */
					let data = [1,2,3,4,5,6,7,8,9,10,11,12,13];
					/* 拿到数据后的处理 */
					if(data.length>0){
						if(type==1){
							this.exampleInfo = data;
						}
						else if(type==2){
							this.exampleInfo = this.exampleInfo.concat(data);
						}
						/* 这里的20是每次调用后台接口返回的最大数据条数，可以自定义 */
						if(this.exampleInfo.length<20){
							this.bottomTips = "nomore";
						}
						else{
							this.bottomTips = "more";
						}
					}
					else{
						if(type==1){
							this.exampleInfo = [];
						}
						else if(type==2){
							this.currentPage--;
						}
						this.bottomTips = "nomore";
					}
					/* 这里300毫秒的延时，主要是为了优化视觉效果 */
					setTimeout(()=>{
						this.$refs.hrPullLoad.reSet();
					},300);
				},500);
			},
			//自定义上拉加载更多
			loadMore(){
				this.currentPage++;
				this.bottomTips = "loading";
				this.getExampleData(2);
			},
			//自定义下拉刷新
			refresh(){
				this.currentPage = 1;
				this.getExampleData(1);
			},
		},
	}
</script>

<style lang="scss" scoped>
	.example{
		width: 100%;
		height: 100%;
		.list{
			width: 100%;
			padding: 24rpx 0rpx;
			display: flex;
			justify-content: space-between;
			align-items: center;
			font-size: 24rpx;
			background: rgb(241,241,241);
			border-bottom: 1px solid rgb(213, 213, 213);
			>view{
				flex: 1;
				text-align: center;
				word-break: break-all;
				display: flex;
				flex-direction: column;
				align-items: center;
				padding: 0 10rpx;
				.num{
					color: #333333;
					font-size: 24rpx;
					font-weight: 500;
				}
				.time{
					color: #999999;
					font-size: 22rpx;
					margin-top: 10rpx;
				}
				.nickName{
					color: #666666;
					font-size: 24rpx;
				}
				.dName,.rank{
					color: #333333;
					font-size: 24rpx;
				}
				.rank{
					margin-top: 10rpx;
				}
			}
			
		}
		.list:nth-last-of-type(1){
			border-bottom: none;
		}
		.examples{
			display: flex;
			justify-content: space-around;
			margin-top: 150rpx;
			>view{
				background-color: rgb(176, 176, 176);
				padding: 20rpx;
				border-radius: 15rpx;
				color: #FFFFFF;
			}
		}
	}
</style>
