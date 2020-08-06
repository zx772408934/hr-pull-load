<template>
	<view class="disciple" v-if="isShow">
			<downRefresh @refresh='refresh' @loadMore='loadMore' :Height='100' :Rate='2' :MaxHeight='100' ref='downRefresh'>
				<view class="noInfo" v-if="discipleInfo.length==0">
					<image src="../../../static/common/noorder.png"></image>
					<text>暂无数据</text>
				</view>
				<view class="havaInfo" v-else>
					<view class="list" v-for="(item,index) in discipleInfo" :key='index'>
						<view class="left">
							<text class="num">{{item.phone_pro}}</text>
							<text class="time">{{item.create_time}}</text>
						</view>
						<view class="middle">
							<text class="nickName">
								{{item.du_nickname}}
							</text>
						</view>
						<view class="right">
							<text class="dName">{{item.name}}</text>
							<text class="rank">LV.{{item.rank}}</text>
						</view>
					</view>
					<uni-load-more :status="loadingType"></uni-load-more>
				</view>
			</downRefresh> 
	</view>
</template>

<script>
	import downRefresh from '../../../components/downRefresh/downRefresh.vue';
	import uniLoadMore from '../../../components/uni-load-more/uni-load-more.vue';
	export default{
		data(){
			return{
				discipleInfo:[],
				loadingType:'more',
				currentPage:1,
				
				isShow:false,
				methods:'',
			}
		},
		components:{
			downRefresh,
			uniLoadMore
		},
		methods:{
			//获取数据
			getDiscipleInfo(type,pre){
				if(pre){
					if(pre !=this.methods){
						this.methods = pre;
						uni.showLoading({
							title:'加载中...',
							mask:true
						});
						this.loadModal = true;
					}
					else{
						return;
					}
				}
				this.$jytools.url_request(this.$url.getMyTeamListV3, {
					type:1,
					page:this.currentPage
				}, 'POST',
					(res) => {
						let data = JSON.parse(res.data);
						this.$emit('showParent');
						if(data.data.length>0){
							if(type==1){
								this.discipleInfo = data.data;
							}
							else if(type==2){
								this.discipleInfo = this.discipleInfo.concat(data.data);
							}
							
							if(this.discipleInfo.length<20){
								this.loadingType = "nomore";
							}
							else{
								this.loadingType = "more";
							}
						}
						else{
							if(type==1){
								this.discipleInfo = [];
							}
							if(type==2){
								this.currentPage--;
							}
							this.loadingType = "nomore";
						}
						this.isShow = true;
						setTimeout(()=>{
							uni.hideLoading();
							this.$refs.downRefresh.reSet();
						},300);
					},
					(error) => {
						this.$emit('showParent');
						this.loadingType = "nomore";
						this.isShow = true;
						setTimeout(()=>{
							this.$refs.downRefresh.reSet();
						},300);
					});
			},
			
			//上拉加载更多
			loadMore(){
				this.currentPage++;
				this.loadingType = "loading";
				this.getDiscipleInfo(2);
			},
			//下拉刷新
			refresh(){
				this.currentPage = 1;
				this.getDiscipleInfo(1);
			},
		},
	}
</script>

<style lang="scss" scoped>
	.disciple{
		width: 100%;
		height: 100%;
		.noInfo{
			width: 100%;
			/* #ifdef APP-PLUS*/
			height: calc(100vh - 116rpx);
			/* #endif */
			
			/* #ifdef H5*/
			height: calc(100vh - 116rpx - 44px);
			/* #endif */
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;
			background: #FFFFFF;
			image {
				margin-top: -150rpx;
				width: 260rpx;
				height: 240rpx;
			}
			
			text {
				color: grey;
			}
		}
		.havaInfo{
			width: 100%;
			padding: 0 30rpx;
			background: #FFFFFF;
			.list{
				width: 100%;
				padding: 24rpx 0rpx;
				display: flex;
				justify-content: space-between;
				align-items: center;
				font-size: 24rpx;
				border-bottom: 1px solid rgb(241,241,241);
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
		}
	}
</style>
