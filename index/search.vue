<template>
	<view class="search">
		<view class="search_head flexs">
			<view class="search_head_ipt flexs">
				<image src="/static/image/home/sousuo@2x.png" mode=""></image>
				<input type="text" @confirm="search" v-model="keyword" placeholder="关键词搜索" placeholder-style="color:#999999" />
			</view>
			<view class="search_head_cancel center" @click="cancelKeyword">取消</view>
		</view>
		<view class="search_box">
			<view class="search_main" v-if="isSearch">
				<view class="search_main_name">搜索历史</view>
				<view class="search_main_ul flexs">
					<view class="search_main_li center" v-for="(item,index) in searchList" :key="index" @click="selectKeyword(item)">{{ item }}</view>
					<view class="search_main_li center" v-if="searchList.length == 0">搜索暂无数据</view>
				</view>
			</view>
			<view class="home_recommend_ul flex" v-else>
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
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				page:1,
				pages:0,
				isSearch:true,
				keyword:'',
				shopList:[],//
				searchList:[],//
			};
		},
		methods:{
			//去详情
			goMessage (item,type) {
				uni.navigateTo({url:'/pages/index/details?id=' + item.box_id + '&type=' + type})
			},
			//关键词搜索
			selectKeyword (item) {
				this.page = 1
				this.isSearch = false
				this.keyword = item
				this.getBoxListByCategory()
			},
			//取消搜索
			cancelKeyword () {
				uni.navigateBack()
				// this.keyword = ''
				// this.isSearch = true
			},
			//搜索
			search () {
				this.page = 1
				this.shopList = []
				this.isSearch = false
				if (!this.keyword) return
				this.getBoxListByCategory()
			},
			getBoxListByCategory () {
				this.$api.search({page:this.page,pagesize:'10',search:this.keyword,msg:'数据加载中'}).then(res=>{
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
						this.getSearch()
						this.shopList = this.page == 1 ? res.data.data : [...rhis.shopList,...res.data.data],
						this.pages = res.data.last_page
						console.log(res.data);
					}
				})
			},
			//获取搜索历史
			getSearch () {
				this.$api.searchHistory().then(res=>{
					if (res.code === 1) {
						this.searchList = res.data.history
					}
				})
			}
		},
		onLoad() {
			this.getSearch()
		}
	}
</script>

<style lang="scss">
.search_head {
	width: 100%;
	height: 88rpx;
	position: sticky;
	top: 0;
	left: 0;
	margin-bottom: 30rpx;
	z-index: 20;
	padding-left: 20rpx;
	background: #FFFFFF;
	.search_head_ipt {
		width: 610rpx;
		height: 58rpx;
		padding: 0 20rpx;
		background: #FAFAFA;
		border-radius: 29rpx;
		image {
			width: 44rpx;
			height: 44rpx;
			margin-right: 20rpx;
		}
		input {
			font-size: 28rpx;
			flex: 1;
		}
	}
	.search_head_cancel {
		flex: 1;
		color: #2F9EFD;
		font-size: 30rpx;
		height: 58rpx;
	}
}
.search_box {
	padding: 0 30rpx;
}
.search_main {
	padding: 0rpx 30rpx;
	background: #FFFFFF;
	border-radius: 22rpx;
	.search_main_name {
		padding: 30rpx 0;
		font-size: 32rpx;
		font-weight: bold;
	}
	.search_main_ul {
		overflow: hidden;
		flex-wrap: wrap;
		.search_main_li {
			height: 60rpx;
			color: #999999;
			font-size: 28rpx;
			padding: 0 33rpx;
			margin: 0 30rpx 30rpx 0;
			background: #FAFAFA;
			border-radius: 30rpx;
		}
	}
	
}
.home_recommend_ul {
	flex-wrap: wrap;
	// padding: 0 28rpx;
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
