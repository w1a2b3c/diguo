<template>
	<view class="home">
		<!--S 头部搜索 -->
		<view class="home_bar">
			<view class="home_bar_nav"></view>
			<view class="home_bar_con flex">
				<view class="home_bar_logo">
					<image :src="logo" mode="aspectFill"></image>
				</view>
				<view class="home_bar_search flexs" @click="goSearch">
					<image src="/static/image/home/sousuo@2x.png" mode=""></image>
					<input type="text" disabled placeholder="输入您要搜索的内容" placeholder-style="color:#808080" />
				</view>
				<view class="home_bar_serve" @click="goServe">
					<image src="/static/image/home/kefu@2x.png" mode=""></image>
				</view>
			</view>
		</view>
		<!--E 头部搜索 -->
		<!--S 轮播图 -->
		<view class="home_swiper">
			<swiper autoplay circular class="swiper">
				<swiper-item v-for="(item,index) in bannerList" :key="index" @click="changeBanner(item)">
					<image :src="item.image" mode="aspectFill"></image>
				</swiper-item>
			</swiper>
			<view class="home_swiper_ul flex">
				<view class="home_swiper_li flexs" v-for="(item,index) in iconList" :key="index">
					<image :src="item.image" mode=""></image>
					<text>{{ item.name }}</text>
				</view>
			</view>
			<view class="home_swiper_nav flex">
				<view class="home_swiper_nav_li center" v-for="(item,index) in navList" :key="index" @click="changeNav(item)">
					<image :src="item.image" mode=""></image>
					<text>{{ item.name }}</text>
				</view>
			</view>
		</view>
		<!--E 轮播图  -->
		<!-- S 试一试 -->
		<!--view class="home_trial flex">
			<view class="home_trial_img flexs">
				<text>试试手气</text>
				<image src="/static/image/home/zuijia@2x.png" mode=""></image>
			</view>
			<view class="home_trial_go" @click="goMessage(0,1)">
				<image src="/static/image/home/jinru@2x.png" mode=""></image>
			</view>
		</view-->
		<!-- E 试一试 -->
		<!-- S 最新开箱 -->
		<!--view class="home_newest">
			<view class="home_newest_head flex">
				<view class="home_newest_head_new">最新开箱</view>
				<view class="home_newest_head_go flexs" @click="goNewBox">
					<text>查看更多</text>
					<image src="/static/image/publice/jinruer@2x.png" mode=""></image>
				</view>
			</view>
			<scroll-view scroll-x="true" class="scroll-view">
				<view class="scroll-view_li" v-for="(item,index) in newBoxList" :key="index" @click="goMessage(item,0)">
					<view class="scroll-view_li_img">
						<image :src="item.goods_images[0]" mode="aspectFill"></image>
					</view>
					<view class="scroll-view_li_price flexs">
						<text>￥</text>
						<text>{{ item.coin_price }}</text>
					</view>
				</view>
			</scroll-view>
			<view class="home_newest_ul flex">
				<view class="home_newest_li flexs" v-for="(item,index) in hotList" :key="index" @click="changeHot(item)">
					<view class="home_newest_img">
						<image :src="item.image" mode=""></image>
					</view>
					<view class="home_newest_con">
						<view class="home_newest_name">{{ item.name }}</view>
						<view class="home_newest_title">{{ item.title }}</view>
					</view>
				</view>
			</view>
		</view-->
		<!-- E 最新开箱 -->
		<!-- S 推荐盲盒 -->
		<view class="home_recommend home_newest">
			<view class="home_newest_head flex">
				<view class="home_newest_head_new">推荐盲盒</view>
				<view class="home_newest_head_go flexs" @click="goRecommend">
					<text>更多分类</text>
					<image src="/static/image/publice/jinruer@2x.png" mode=""></image>
				</view>
			</view>
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
		<!-- E 推荐盲盒 -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
				iconList:[
					{
						image:'/static/image/home/zhengpinbaozheng@2x.png',
						name:'官方正品保证'
					},
					{
						image:'/static/image/home/yijianhuishou@2x.png',
						name:'一键秒回收'
					},
					{
						image:'/static/image/home/youhui@2x.png',
						name:'全网最优惠'
					}
				],
				navList:[
					{
						image:'/static/image/home/chaowanmanghe@2x.png',
						name:'欢乐拆盒',
						url:'/pages/index/more'
					},
					{
						image:'/static/image/home/youhuichongzhi@2x.png',
						name:'优惠充值',
						url:'/pages/me/recharge'
					},
					{
						image:'/static/image/home/fahuoliucheng@2x.png',
						name:'发货流程',
						url:'/pages/index/flow'
					},
					{
						image:'/static/image/home/xinshoubangzhu@2x.png',
						name:'新手帮助',
						url:'/pages/index/help'
					},
				],//导航列表
				hotList:[
					{
						image:'/static/image/home/remen@2x.png',
						name:'热门专区',
						title:'跟随热门手气更欧',
						url:'/pages/index/hot?type=0'
					},
					{
						image:'/static/image/home/dijia@2x.png',
						name:'低价专区',
						title:'低价试试手气',
						url:'/pages/index/hot?type=1'
					}
				],
				bannerList:[],//轮播图
				logo:'',//logo
				shopList:[],//获取推荐列表
				newBoxList:[],//最新开箱
				pages:0,
				page:1,//
			};
		},
		methods:{
			//去客服
			goServe () {
				uni.navigateTo({url:'/pagesB/pages/server'})
			},
			//去推荐，盲盒
			goRecommend () {
				uni.navigateTo({url:'/pages/index/recommend'})
			},
			//最新开箱
			goNewBox () {
				uni.navigateTo({url:'/pages/index/newBox'})
			},
			//轮播图
			changeBanner (item) {
				if (item.type == "word") {
					uni.setStorageSync('bannerMessage',JSON.stringify(item.value))
					uni.navigateTo({url:'/pagesA/pages/bannerMessage'})
				} else if (item.type == 'link') {
					location.href = item.value
				} else if (item.type == 'box'){
					uni.navigateTo({url:'/pages/index/details?id=' + item.value + '&type=0'})
				}
			},
			//去搜索
			goSearch () {
				uni.navigateTo({url:'/pages/index/search'})
			},
			//去详情
			goMessage (item,type) {
				uni.navigateTo({url:'/pages/index/details?id=' + item.box_id + '&type=' + type})
			},
			//热门专区
			changeHot (item) {
				uni.navigateTo({url:item.url})
			},
			//头部四个
			changeNav (item) {
				uni.navigateTo({url:item.url})
			},
			//获取轮播图
			getbannerList () {
				
			},
			//最新开箱
			getNewbox () {
				this.$api.newestOpen().then(res=>{
					if (res.code === 1) {
						this.newBoxList = res.data
					}
				})
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
			//获取首页信息
			getBaseInfo () {
				this.$api.baseInfo().then(res=>{
					if (res.code === 1) {
						this.logo = res.data.logo
						this.bannerList = res.data.banner
					}
				})
			}
		},
		onLoad({sharecode,token}) {
			if (sharecode) {
				uni.setStorageSync('sharecode',sharecode)
			}
			if (token) {
				uni.setStorageSync('token',token)
			}
			this.getBaseInfo()
			this.getbannerList()
			this.getNewbox()
			this.getShopList()
		},
		onReachBottom () {
			if (this.page < this.pages) {
				this.page++
				this.getShopList()
			}
		}
	}
</script>

<style lang="scss">
.home_bar {
	width: 100%;
	position: sticky;
	top: 0;
	left: 0;
	padding: 0 30rpx;
	z-index: 2021;
	background: #FFFFFF;
	.home_bar_nav {width: 100%;}
	.home_bar_con {
		height: 88rpx;
	}
	.home_bar_logo {
		width: 66rpx;
		height: 66rpx;
	}
	.home_bar_search {
		width: 520rpx;
		height: 58rpx;
		padding: 0 20rpx;
		background: #F7F7F7;
		border-radius: 29rpx;
		image {
			width: 44rpx;
			height: 44rpx;
		}
		input {
			flex: 1;
			font-size: 26rpx;
			margin-left: 15rpx;
		}
	}
	.home_bar_serve {
		width: 44rpx;
		height: 44rpx;
	}
}
.home_swiper {
	padding: 0 30rpx;
	background: #FFFFFF;
	.swiper {
		height: 266rpx;
		image {
			border-radius: 20rpx;
		}
	}
	.home_swiper_li {
		height: 64rpx;
		image {
			width: 34rpx;
			height: 34rpx;
			margin-right: 10rpx;
		}
		text {
			color: #666666;
		}
	}
	.home_swiper_nav_li {
		height: 150rpx;
		flex-direction: column;
		image {
			width: 70rpx;
			height: 70rpx;
			margin-bottom: 10rpx;
		}
		text {
			color: #000000;
			font-size: 26rpx;
		}
	}
}
.home_trial {
	height: 88rpx;
	margin: 10rpx 0;
	padding: 0 30rpx;
	background: #FFFFFF;
	.home_trial_img {
		text {
			font-size: 28rpx;
		}
		image {
			width: 38rpx;
			height: 38rpx;
			margin-left: 10rpx;
		}
	}
	.home_trial_go {
		width: 50rpx;
		height: 50rpx;
	}
}
.home_newest {
	margin-bottom: 10rpx;
	background: #FFFFFF;
	.home_newest_head {
		padding: 30rpx 28rpx;
		.home_newest_head_new {
			font-size: 28rpx;
			font-weight: bold;
		}
		.home_newest_head_go {
			text {
				color: #999999;
			}
			image {
				width: 22rpx;
				height: 22rpx;
				margin-left: 10rpx;
			}
		}
	}
	.scroll-view {
		white-space:nowrap;
		box-sizing: border-box;
		padding-left: 28rpx;
		.scroll-view_li {
			margin-right: 20rpx;
			display: inline-block;
			.scroll-view_li_img {
				image{
					width: 150rpx;
					height: 150rpx;
					border-radius: 10rpx;
				}
				margin-bottom: 20rpx;
			}
			.scroll-view_li_price {
				text {
					color: #CF271B;
					&:last-child {
						font-size: 30rpx;
					}
				}
			}
		}
	}
	.home_newest_ul {
		padding: 36rpx 28rpx 24rpx 28rpx;
		.home_newest_li {
			width: 332rpx;
			height: 130rpx;
			padding-left: 30rpx;
			background: #F8FAFB;
			border-radius: 6rpx;
			.home_newest_img {
				width: 70rpx;
				height: 70rpx;
				margin-right: 24rpx;
			}
			.home_newest_name {
				font-size: 30rpx;
				font-weight: bold;
			}
			.home_newest_title {
				color: #999999;
				margin-top: 10rpx;
			}
		}
	}
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
