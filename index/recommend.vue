<template>
	<view class="recommend">
		<view class="home_recommend_ul flex">
			<view class="home_recommend_li" v-for="(item,index) in shopList" :key="index" @click="goMessage(item,0)">
				<view class="home_recommend_li_img">
					<view class="home_recommend_li_img_top flex">
						<view class="home_recommend_li_img_box">
							<image :src="item.image" mode="aspectFill"></image>
						</view>
						<view class="home_recommend_li_img_right" v-if="item.right.length != 0">
							<image :src="items" mode="aspectFill" v-for="(items,index) in item.right" :key="index"></image>
						</view>
					</view>
					<view class="home_recommend_li_img_bot">
						<image :src="items" mode="aspectFill" v-for="(items,index) in  item.bot" :key="index"></image>
					</view>
				</view>
				<view class="home_recommend_li_main">
					<view class="home_recommend_li_price">价值:￥{{ item.price_min }}~￥{{ item.price_max }}</view>
					<view class="home_recommend_li_name">{{ item.box_name }}</view>
					<view class="home_recommend_li_glod flex">
						<text>{{ item.coin_price }}金币</text>
						<text>共{{ item.goods_num }}款商品</text>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				page:1,
				pages:0,//
				shopList:[],//推荐列表
			};
		},
		methods:{
			//去详情
			goMessage (item,type) {
				uni.navigateTo({url:'/pages/index/details?id=' + item.box_id + '&type=' + type})
			},
			//获取推荐列表
			getShopList () {
				this.$api.recommend().then(res=>{
					if (res.code === 1) {
						res.data.data.forEach(item=>{
							item.image = item.goods_images[0]
							item.right = []
							item.bot = []
							item.goods_images.forEach((em,index)=>{
								if (index > 0 && index < 3) {
									item.right.push(em)
								}
								if (index > 2) {
									item.bot.push(em)
								}
							})
						})
						this.shopList = this.page == 1 ? res.data.data : [...this.shopList,...res.data.data],
						this.pages = res.data.last_page
					}
				})
			},
		},
		onLoad() {
			this.getShopList()
		}
	}
</script>

<style lang="scss">
	.recommend {
		padding-top: 30rpx;
	}
.home_recommend_ul {
	flex-wrap: wrap;
	padding: 0 28rpx;
	.home_recommend_li {
		width: 332rpx;
		padding: 20rpx;
		margin-bottom: 30rpx;
		background: #FFFFFF;
		box-shadow: 0rpx 0rpx 10rpx 0rpx rgba(153, 153, 153, 0.1);
		border-radius: 10rpx;
		.home_recommend_li_img {
			height: 295rpx;
			.home_recommend_li_img_top {
				align-items: flex-start;
			}
			.home_recommend_li_img_bot {
				display: flex;
				margin-top: 20rpx;
				justify-content: flex-end;
				image {
					width: 85rpx;
					height: 85rpx;
					margin-left: 20rpx;
					&:first-child {
						margin-left: 0;
					}
					border-radius: 6rpx;
				}
			}
			.home_recommend_li_img_box {
				image {
					width: 190rpx;
					height: 190rpx;
					border-radius: 6rpx;
				}
			}
			.home_recommend_li_img_right {
				image {
					width: 85rpx;
					height: 85rpx;
					border-radius: 6rpx;
					&:first-child {
						margin-bottom: 20rpx;
					}
				}
			}
		}
		.home_recommend_li_price {
			// width: 280rpx;
			height: 40rpx;
			color: #FFFFFF;
			margin: 20rpx 0;
			font-size: 26rpx;
			line-height: 40rpx;
			padding-left: 18rpx;
			background: #F6AF32;
			border-radius: 20rpx;
		}
		.home_recommend_li_name {
			font-size: 28rpx;
			display: -webkit-box;
			-webkit-box-orient: vertical;
			-webkit-line-clamp: 2;
			overflow: hidden;
			height: 76rpx;
		}
		.home_recommend_li_glod {
			margin-top: 30rpx;
			text {
				color: #CF271B;
				font-size: 26tpx;
				&:last-child {
					color: #666666;
					font-size: 24rpx;
				}
			}
		}
	}
}
</style>
