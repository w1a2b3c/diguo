<!--  -->
<template>
	<view class="details">
		<view class="details_title">
			<view class="details_nav"></view>
			<view class="details-title_head flex">
				<view class="details_title_return" @click="returnTop">
					<image src="/static/image/publice/fanhui@2x.png" mode=""></image>
				</view>
				<view class="details_title_name">{{ boxDetail.box_name }}</view>
				<view class="details_title_mp3" @click="changePlay">
					<image :src="isPlay ? '/static/image/home/bofang@2x.png' : '/static/image/home/guanbiyinyue@2x.png' " mode=""></image>
				</view>
			</view>
		</view>
		<!--S 头部 -->
		<view class="details_head">
			<view class="details_head_title flex">
				<view class="details_head_price flexs">
					<view class="details_head_gold">我的余额：{{ boxDetail.mycoin }}金币</view>
					<view class="details_head_btn center" @click="recharge">充值</view>
				</view>
				<view class="details_head_rule flexs" @click="$refs.regulation.open()">
					<text>玩法规则</text>
					<image src="/static/image/home/wenhao@2x.png" mode=""></image>
				</view>
				<view class="details_head_shop center">
					<swiper autoplay circular class="swiper">
						<swiper-item v-for="(item,index) in boxDetail.box_banner" :key="index">
							<image :src="item.image" mode="aspectFill" @click="lookImg(item.image)"></image>
							<text>{{ item.desc }}</text>
						</swiper-item>
					</swiper>
					<!-- <image src="/static/image/home/erji@2x.png" mode=""></image> -->
					
				</view>
			</view>
		</view>
		<!--E 头部 -->
		<!--S 商品列表 -->
		<view class="details_shop">
			<view class="details_shop_head flex">
				<view class="details_shop_head_l">可出商品列表</view>
				<!-- <view class="details_shop_head_go flexs" @click="goMore">
					<text>更多</text>
					<image src="/static/image/publice/jinruer@2x.png" mode=""></image>
				</view> -->
			</view>
			<view class="details_shop_ul flexs">
				<view class="details_shop_li" v-for="(item,index) in boxDetail.goodslist" :key="index">
					<view class="shopList_li_tag center" :class="{orange:item.tag == '稀有',red:item.tag == '至尊'}">{{ item.tag }}</view>
					<view class="details_shop_li_img center">
						<image :src="item.image" mode="aspectFill"  @click="lookImg(item.image)"></image>
						<view class="details_shop_li_price ">
							<text>￥</text>
							<text>{{ item.price }}</text>
						</view>
					</view>
					<view class="details_shop_li_name">{{ item.goods_name }}</view>
				</view>
			</view>
		</view>
		<!--E 商品列表 -->
		<!--S 开箱记录 -->
		<!--view class="details_record">
			<view class="details_record_head">开箱记录</view>
			<view class="details_record_ul">
				<view class="details_record_li flexs" v-for="(item,index) in boxDetail.record" :key="index">
					<view class="details_record_li_img">
						<image :src="item.goods_image" mode="aspectFill"></image>
					</view>
					<view class="details_record_li_mian">
						<view class="details_record_li_head flexs">
							<image :src="item.avatar" mode="aspectFill"></image>
							<view class="details_record_li_main_txt">
								<text>{{ item.nickname }}</text><text class="gary">开出</text><text>{{ item.goods_name }}</text>
							</view>
						</view>
						<view class="details_record_li_jia flexs">
							<view class="details_record_li_jia_txt">价值：</view>
							<view class="details_record_li_jia_price">
								<text>￥</text>
								<text>{{ item.goods_rmb_price }}</text>
							</view>
						</view>
						<view class="details_record_li_time">开盒时间：{{ item.create_time }}</view>
					</view>
				</view>
			</view>
		</view-->
		<!--E 开箱记录 -->
		<!-- S 底部 -->
		<view class="details_footer flex" :style="{'justify-content':type == 1 ? 'center' : ''}">
			<view class="details_footer_coll" @click="changeStar" v-if="type == 0">
				<image :src=" boxDetail.is_star == 1 ? '/static/image/home/shoucangyixuanzhong@2x.png' : '/static/image/home/shoucangweixuanzhong@2x.png' " mode=""></image>
			</view>
			<view class="details_footer_ul center">
				<view class="details_footer_li center" v-for="(item,index) in 2" :key="index" @click="changeDraw(index)">{{ index == 0 ? (type == 1 ? '试玩一发入魂' : boxDetail.coin_price + '金币一发入魂') :( type == 1 ? '试玩五连绝世' : Number(boxDetail.coin_price) * 5 + '金币五连绝世') }}</view>
			</view>
		</view>
		<!--E 底部  -->
		<!--S  玩法规则 -->
		<uni-popup ref="regulation" :mask-click="false">
		  <view class="regulation">
				<view class="regulation_close" @click="$refs.regulation.close()">
					<image src="/static/image/home/guanbi@2x.png" mode=""></image>
				</view>
		  	<view class="regulation_name">玩法规则</view>
				<view class="regulation_main">
					<scroll-view scroll-y="true" class="scroll-view">
						<u-parse :content="message" ></u-parse>
					</scroll-view>
				</view>
		  </view>
		</uni-popup>
		
		<!--E  玩法规则 -->
	</view>
</template>

<script>
	const innerAudioContext = uni.createInnerAudioContext();
	innerAudioContext.autoplay = true;
	innerAudioContext.loop = true;
	innerAudioContext.src = '/static/image/home/bj.mp3';
	
	export default {
		data() {
			return {
				type:null,//1试玩0详情
				box_id:'',//盲盒ID
				message:'',//玩法规则
				boxDetail:{},//盲盒详情
				isPlay:true,
			};
		},
		methods:{
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
			changePlay () {
				this.isPlay = !this.isPlay
				uni.setStorageSync('isPlay',this.isPlay)
				this.isPlay ? innerAudioContext.play() : innerAudioContext.pause()
			},
			//去充值
			recharge () {
				uni.navigateTo({url:'/pages/me/recharge'})
			},
			//去抽奖
			changeDraw (index) {
				innerAudioContext.pause()
				uni.navigateTo({url:'/pagesA/pages/camera?type=' + this.type + '&index=' + index + '&id=' + (this.type== 1 ? this.boxDetail.box_id : this.box_id) + '&name=' + this.boxDetail.box_name})
			},
			//是否点赞
			changeStar () {
				if (this.type == 1) return uni.showToast({title:'试玩商品不可操作',icon:'none'})
				this.$api.star({box_id:this.box_id}).then(res=>{
					if (res.code === 1) {
						res.data.is_star == 1 ? this.boxDetail.is_star = 1 : this.boxDetail.is_star = 0
						uni.showToast({title:res.msg})
					}
				})
			},
			//去更多商品
			goMore () {
				uni.navigateTo({url:'/pages/index/shopList?id=' + this.box_id + '&type=' + this.type})
			},
			returnTop () {
				uni.navigateBack()
			},
			//获取详情
			getDetails () {
				this.$api[this.type == 1 ? 'tryBoxDetail' : 'boxDetail' ]({box_id:this.box_id}).then(res=>{
					if (res.code === 1) {
						this.boxDetail = res.data
					}
				})
			},
			//玩法规则
			getMessage () {
				this.$api.agreement({name:'play_rule'}).then(res=>{
					if (res.code === 1) {
						this.message = res.data.content
					}
				})
			}
		},
		onUnload() {
			innerAudioContext.pause()
		},
		onLoad({id,type}) {
			this.type = type
			this.box_id = id
			this.getDetails()
			this.getMessage()
		},
		onShow() {
			this.isPlay = uni.getStorageSync('isPlay')
			this.isPlay ? innerAudioContext.play() : innerAudioContext.pause()
		},
	}
</script>

<style lang="scss">
	.scroll-view {
		height: 735rpx;
	}
	.details {
		padding-bottom: 98rpx;
	}
.details_title {
	width: 100%;
	position: sticky;
	top: 0;
	z-index: 2021;
	.details_nav {width: 100%;}
	.details-title_head {
		width: 100%;
		height: 88rpx;
		padding: 0 30rpx;
		background: #FFFFFF;
		.details_title_return {
			width: 44rpx;
			height: 44rpx;
		}
		.details_title_name {
			width: 466rpx;
			text-align: center;
			overflow: hidden;
			text-overflow:ellipsis;
			white-space: nowrap;
			font-size: 36rpx;
			font-weight: bold;
		}
		.details_title_mp3 {
			width: 44rpx;
			height: 44rpx;
		}
	}
}
.details_head {
	height: 657rpx;
	position: relative;
	background: url(../../static/image/home/beijing@2x.png) no-repeat;
	background-size: cover;
	.details_head_title {
		padding: 30rpx;
		
		.details_head_gold {
			color: #FFFFFF;
			font-size: 30rpx;
			font-weight: bold;
		}
		.details_head_btn {
			width: 120rpx;
			height: 50rpx;
			color: #FFFFFF;
			font-size: 28rpx;
			background: linear-gradient(0deg, #F6AF32 0%, #F7751F 100%);
			border-radius: 25rpx;
			margin-left: 30rpx;
		}
		.details_head_rule {
			image {
				width: 24rpx;
				height: 24rpx;
				margin-left: 10rpx;
			}
			text {
				color: #FFFFFF;
				font-size: 26rpx;
			}
		}
		.details_head_shop {
			display: flex;
			top: 272rpx;
			left: 50%;
			width: 100%;
			transform: translateX(-50%);
			position: absolute;
			flex-direction: column;
			.swiper {
				width: 200rpx;
				height: 270rpx;
				swiper-item {
					display: flex;
					align-items: center;
					justify-content: space-between;
					flex-direction: column;
				}
			}
			image {
				width: 120rpx;
				height: 163rpx;
			}
			text {
				color: #FFFFFF;
				font-size: 28rpx;
				// margin-top: 107rpx;
			}
		}
	}
}
.details_shop {
	position: relative;
	border-radius: 30rpx 30rpx 0rpx 0rpx;
	padding: 0 30rpx;
	margin-top: -30rpx;
	background: #FFFFFF;
	.details_shop_head {
		padding: 30rpx 0;
		
		.details_shop_head_l {
			font-size: 30rpx;
			font-weight: bold;
		}
		.details_shop_head_go {
			image {
				width: 22rpx;
				height: 22rpx;
				margin-left: 10rpx;
			}
			text {
				color: #999999;
			}
		}
	}
	.details_shop_ul {
		flex-wrap: wrap;
		.details_shop_li {
			position: relative;
			margin-right: 30rpx;
			margin-bottom: 30rpx;
			.shopList_li_tag {
				top: 0;
				left: 0;
				color: #FFFFFF;
				font-size: 22rpx;
				width: 80rpx;
				height: 36rpx;
				z-index: 20;
				position: absolute;
				background:rgba(0, 0, 0, 0.5);
				border-radius: 10rpx 0rpx 20rpx 0rpx;
			}
			.orange {
				background: #FA7E48;
			}
			.red {
				background: #FF413F;
			}
			&:nth-child(3n) {
				margin-right: 0;
			}
			.details_shop_li_name {
				width: 210rpx;
				height: 68rpx;
				display: -webkit-box;
				-webkit-box-orient: vertical;
				-webkit-line-clamp: 2;
				overflow: hidden;
				font-size: 26rpx;
			}
			.details_shop_li_img{
				width: 210rpx;
				height: 204rpx;
				flex-direction: column;
				margin-bottom: 20rpx;
				background: #FEF9E5;
				border-radius: 10rpx;
			}
			image {
				width: 135rpx;
				height: 135rpx;
				margin-bottom: 10rpx;
			}
			.details_shop_li_price {
				display: flex;
				justify-content: center;
				text {
					color: #CF271B;
					&:last-child {
						font-size: 28rpx;
					}
				}
			}
		}
	}
}
.details_record {
	padding: 0 30rpx;
	.details_record_head {
		padding: 30rpx 0;
		font-size: 30rpx;
		font-weight: bold;
	}
	.details_record_li {
		padding: 30rpx;
		margin-bottom: 10rpx;
		background: #FFFFFF;
		border-radius: 20rpx;
		.details_record_li_img {
			image {
				width: 168rpx;
				height: 168rpx;
				border-radius: 10rpx;
			}
			margin-right: 20rpx;
		}
	}
	.details_record_li_head {
		image {
			width: 68rpx;
			height: 68rpx;
			flex-shrink: 0;
			margin-right: 20rpx;
			border-radius: 34rpx;
		}
		.details_record_li_main_txt {
			text {
				color: #10ADEE;
				font-size: 26rpx;
			}
			.gary {
				margin: 0 10rpx;
				color: #333333;
			}
		}
	}
	.details_record_li_jia {
		margin: 20rpx 0 10rpx 0;
	}
	.details_record_li_jia_txt {
		font-size: 24rpx;
		margin-right: 14rpx;
	}
	.details_record_li_jia_price {
		display: flex;
		align-items: flex-end;
		text {
			color: #CF271B;
			&:last-child {
				font-size: 30rpx;
			}
		}
	}
	.details_record_li_time {
		color: #999999;
	}
}
.details_footer {
	width: 100%;
	height: 98rpx;
	position: fixed;
	bottom: 0;
	left: 0;
	z-index: 2021;
	padding: 0 26rpx 0 46rpx;
	background: #FFFFFF;
	.details_footer_coll {
		width: 54rpx;
		height: 54rpx;
	}
	.details_footer_li {
		width: 284rpx;
		height: 78rpx;
		margin-left: 15rpx;
		color: #FFFFFF;
		font-size: 26rpx;
		margin-left: 15rpx;
		padding-bottom: 10rpx;
		background: url(../../static/image/home/yifaruhun@2x.png) no-repeat;
		background-size: cover;
		&:last-child {
			background: url(../../static/image/home/wulianjueshi@2x.png) no-repeat;
			background-size: cover;
		}
		
	}
}
.regulation {
	width: 650rpx;
	height: 900rpx;
	padding: 0 0 0 30rpx;
	background: #FFFFFF;
	border-radius: 20rpx;
	position: relative;
	.regulation_close {
		position: absolute;
		width: 44rpx;
		height: 44rpx;
		top: 30rpx;
		right: 30rpx;
	}
	.regulation_name {
		padding: 40rpx 0;
		text-align: center;
		font-size: 30rpx;
		font-weight: bold;
	}
}

</style>
