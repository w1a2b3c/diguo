
<!-- 欢乐拆盒 -->
<template>
	<view class="hot">
		<view class="hot_swiper">
			<image :src="image" mode="aspectFill"></image>
			<!-- <swiper autoplay circular class="swiper">
				<swiper-item v-for="(item,index) in 3" :key="index">
					<image src="/static/logo.png" mode=""></image>
				</swiper-item>
			</swiper> -->
		</view>
		<view class="home_recommend_ul flex">
			<view class="home_recommend_li" v-for="(item,index) in hotList" :key="index" @click="goMessage(item,0)">
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
				type:0,//热门还是低价
				image:'',//上方图片
				page:1,
				pages:null,//总页数
				hotList:[],//热门列表
			};
		},
		methods:{
			//去详情
			goMessage (item,type) {
				uni.navigateTo({url:'/pages/index/details?id=' + item.box_id + '&type=' + type})
			},
			//获取热门列表
			getHotList () {
				this.$api[this.type == 0 ? 'hotBox' : 'cheapBox']({page:this.page,msg:'数据加载中'}).then(res=>{
					if (res.code === 1) {
						res.data.list.data.forEach(item=>{
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
						this.image = res.data.banner
						this.hotList = this.page == 1 ? res.data.list.data : [this.hotList,...res.data.list.data]
						this.pages = res.data.list.last_page
						
					}
				})
			}
		},
		onLoad({type}) {
			this.type = type
			this.getHotList()
		}
	}
</script>

<style lang="scss">
	.hot_swiper {
		height: 320rpx;
	}
.swiper {
	height: 320rpx;
	margin-bottom: 30rpx;
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
			line-height: 76rpx;
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
