<template>
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	<view class="box">
		
		
		<view class="box_ul">
			<view class="box_ul_li " v-for="(item,index) in boxList" :key="index"  @click="item.flag = !item.flag">
			
			 
			
			<view class="s-bt flex">
			<view class="s-tx">	<image :src="item.box_tx" mode="aspectFill"></image>
			</view>
			
				<view class="s-mc">{{ item.box_mc}}</view>
					<!-- <view class="s-sj">{{ item.box_sj}}</view> -->
					<view class="s-sj">本周</view>
			<view class="s-qc"></view>
			</view>
			
			<view class="s-zw">{{ item.box_zw}}</view>
			
			
			
			<view class="s-img-list ">
			<view class="s-img-box"  v-for="(item2,index) in item.box_img" :key="index"  @click="item.flag = !item.flag">
				
				<image :src="item2"   mode="aspectFill" @click="lookImg(item2)"></image>
				</view>
			 	<view class="s-qc"></view>
				
				
			</view>
			
			
			
			</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				i:0,//
				page:1,
				isAll:false,//
				pages:null,//
				totalNumber:0,//总数量
				boxList:[],//盒子列表
			};
		},
		watch:{
			boxList:{
				handler(val){
					let totalNumber = 0
					
					this.isAll = val.every(item=>item.flag)
					if (val.length == 0) this.isAll = false
					val.forEach(item=>{
						if (item.flag) {
							totalNumber++
						}
					})
					this.totalNumber = totalNumber
				},
				deep:true //true 深度监听
			}
		},
		methods:{
			//申请发货
			apply (index) {
				if (!this.boxList.some(item=>item.flag)) return uni.showToast({title:'请勾选要' +( index == 0 ? '回收' : '发货') +  '的商品',icon:'none'})
				let ids = []
				this.boxList.forEach(item=>{
					if (item.flag) {
						ids.push(item.record_id)
					}
				})
				if (index == 0) {
					this.$api.exchange({record_ids:ids.join(',')}).then(res=>{
						if (res.code === 1) {
							uni.showToast({title:res.msg})
							this.page = 1
							this.getMyBox()
						}
					})
				} else {
					uni.navigateTo({url:'/pages/me/applyFaHuo?id=' + ids.join(',')})
				}
			},
			
			
			lookImg (url) {
				uni.previewImage({
						urls: [url],
						longPressActions: {
								itemList: ['发送给朋友', '保存图片', '收藏'],
								success: function(data) {
										console.log('选中了第' + (data.tapIndex + 1) + '个按钮,第' + (data.index + 1) + '张图片');
								},
								fail: function(err) {
										console.log(err.errMsg);
								}
						}
				});
			},
			//全选
			changeAll () {
				this.isAll = !this.isAll
				this.boxList.forEach(item=>{
					item.flag = this.isAll
				})
			},
			changeIndex (index) {
				this.i = index
				this.page = 1
				this.boxList = []
				this.getMyBox()
			},
			//获取我的盒柜
			getMyBox () {
				this.$api.sList({status:this.i + 1,page:this.page,msg:'数据加载中'}).then(res=>{
					if (res.code === 1) {
						res.data.data.forEach(item=>{
							item.flag = false
						})
						this.pages = res.data.last_page
						this.boxList = this.page == 1 ? res.data.data : [...this.boxList,...res.data.data]
					}
				})
			}
		},
		
		
		
		
		onShow () {
			this.getMyBox()
		},
		onReachBottom() {
			if (this.page < this.pages) {
				this.page++
				this.getMyBox()
			}
		}
	}
</script>

<style lang="scss">
	
	.s-bt { display: block;}
	.s-tx{ height:80rpx; 
	width: 80rpx;
	float: left;
	image {
		border-radius: 40rpx; 
		
		
	}
	 
	 }
	 
	 .s-mc {
		 float: left;
		 line-height: 80rpx; padding-left: 30rpx;
		 font-size: 26rpx;
		 
	 }
	 .s-sj {
		 float: right;
		  line-height: 80rpx;
		  padding-right:10rpx; color:#999;
	 }
	 .s-qc {
		 clear: both;
	 }
	 
	 
	 .s-zw {
		 padding: 20rpx 0;
		     -webkit-box-pack: justify;
		     -webkit-justify-content: space-between;
		     justify-content: space-between;
		     color: var(--mainColor);
		     font-size: 16px;
		     text-align: justify;
			 line-height: 1.6;
		 
	 }
	 
	 
	 
	 
	 
	 .s-img-box {
	 	float: left;
		
	
		
		
		image {
			width: 190rpx;
			height: 120rpx;
			margin-bottom: 16rpx;
			margin-left:16rpx;
		}
		text {
			color: #000000;
			font-size: 26rpx;
		}
		
		
		}
	
.box_head {
	position: sticky;
	top: 88rpx;
	left: 0;
	background: #FAFAFA;
	z-index: 2021;
	width: 100%;
	.box_head_li {
		flex:1 ;
		font-size: 30rpx;
		height: 90rpx;
		font-weight: bold;
	}
	.active {
		color: #F6AF32;
	}
}
.box_ul {
	padding: 0 30rpx 98rpx 30rpx;
	.box_ul_li {
		padding: 30rpx;
		margin-bottom: 20rpx;
		background: #FFFFFF;
		border-radius: 20rpx;
		.box_ul_li_img {
			image {
				width: 168rpx;
				height: 168rpx;
				border-radius: 10rpx;
			}
			margin-right: 20rpx;
		}
		.box_ul_li_main {
			flex: 1;
		}
		.box_ul_li_name {
			font-size: 28rpx;
			display: -webkit-box;
			-webkit-box-orient: vertical;
			-webkit-line-clamp: 2;
			overflow: hidden;
		}
		.box_ul_li_price {
			text {
				color: #CF271B;
				font-size: 28rpx;
			}
			image {
				width: 44rpx;
				height: 44rpx;
			}
			margin: 15rpx 0;
		}
		.box_ul_li_time {
			color: #999999;
			font-size: 22rpx;
		}
	}
}
.box_footer {
	height: 98rpx;
	z-index: 20;
	position: fixed;
	bottom: 50px;
	width: 100%;
	padding: 0 30rpx;
	background: #FFFFFF;
	.box_footer_all {
		image {
			width: 44rpx;
			height: 44rpx;
			margin-right: 10rpx;
		}
		text {
			color: #000000;
			font-size: 28rpx;
		}
	}
	.box_footer_li {
		color: #FFFFFF;
		width: 220rpx;
		height: 78rpx;
		border-radius: 39rpx;
		font-size: 30rpx;
		margin-left: 30rpx;
		&:first-child {
			background: #FA7E48;
		}
		&:last-child {
			background: #F6AF32;
		}
	}
}
</style>
