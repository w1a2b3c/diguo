<!-- 欢乐拆盒-->
<template>
	<view class="more">
		<view class="more_head flex">
			<view class="more_head_li flexs" :class="{active:index == i}" v-for="(item,index) in list" :key="index" @click="changeHead(index)">
				<text>{{ item.name }}</text>
				<image src="/static/image/home/xiala@2x.png" :class="{handstand:item.type == 1}" mode="" v-if="index != 0"></image>
			</view>
		</view>
		<view class="home_recommend_ul flex">
			<view class="home_recommend_li" v-for="(item,index) in boxList" :key="index" @click="goMessage(item,0)">
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
		<view class="screen" :style="{width:isShow ? '600rpx' : 0}">
			<view class="screen_box">
				<view class="screen_name">价格区间</view>
				<view class="screen_head flexs">
					<view class="screen_ipt center">
						<input type="number" v-model="minPrice" placeholder="最低价" placeholder-style="color:#999999" />
					</view>
					<view class="screen_head_line"></view>
					<view class="screen_ipt center">
						<input type="number" v-model="maxPrice" placeholder="最高价" placeholder-style="color:#999999" />
					</view>
				</view>
				<view class="screen_ul flexs">
					<view class="screen_li center" :class="{active:index == selectPrice}" v-for="(item,index) in priceList" :key="index" @click="selectprice(index)">{{ item }}</view>
				</view>
			</view>
			<view class="screen_footer flex">
				<view class="screen_footer_li center" v-for="(item,index) in 2" :key="index" @click="reset(index)">{{ index == 0 ? '重置' : '确定' }}</view>
			</view>
		</view>
		<view class="marking" v-if="isShow"></view>
		<!-- <uni-popup ref="popup" :mask-click="false" type="right">
		  
		</uni-popup> -->
		<!-- <uni-drawer ref="popup" mode="right" :mask-click="false">
				<scroll-view style="height: 100%;" scroll-y="true">
						<button @click="closeDrawer" type="primary">关闭Drawer</button>
						<view v-for="item in 60" :key="item">可滚动内容 {{ item }}</view>
				</scroll-view>
		</uni-drawer> -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
				i:-1,
				page:1,
				pages:0,
				price:'',//加个排序
				sort:'',//排序
				list:[{name:'新品',type:0},{name:'综合',type:0},{name:'价格',type:0},{name:'筛选',type:0}],
				boxList:[],//
				isShow:false,
				selectPrice:-1,//选择哪个价钱
				minPrice:'',//最低价
				maxPrice:'',//最高价
				priceList:[],//价格区间
			};
		},
		methods:{
			//去详情
			goMessage (item,type) {
				uni.navigateTo({url:'/pages/index/details?id=' + item.box_id + '&type=' + type})
			},
			//重置
			reset (index) {
				if (index == 0) {
					this.minPrice = ''
					this.maxPrice = ''
					this.selectPrice = -1
				} else {
					this.price = ((this.minPrice == 0 || this.minPrice) && this.maxPrice) ? this.minPrice + '-' + this.maxPrice : ''
					this.isShow = false
					this.list[3].type = 0
					this.getBoxListByCategory()
				}
			},
			//选择价钱
			selectprice (index) {
				this.selectPrice = index == this.selectPrice ? -1 : index
				if (this.selectPrice != -1) {
					this.minPrice = this.priceList[index].split('-')[0]
					this.maxPrice = this.priceList[index].split('-')[1]
				}
			},
			changeHead (index) {
				this.page = 1
				this.i = index
				this.list[index].type = this.list[index].type == 0 ? 1 : 0
				if (index == 0) {
					this.sort = 'new'
				} else if (index == 1){
					this.sort = this.list[index].type == 0 ? 'combine_desc' : 'combine_asc'
				} else if (index == 2) {
					this.sort = this.list[index].type == 0 ? 'price_desc' : 'price_asc'
				} else {
					this.list[index].type == 1 ? this.isShow = true : this.isShow = false
					return
				}
				this.getBoxListByCategory()
			},
			returnTop () {
				uni.navigateBack()
			},
			getBoxListByCategory () {
				this.$api.search({page:this.page,msg:'数据加载中',sort:this.sort,price:this.price}).then(res=>{
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
						this.boxList = this.page == 1 ? res.data.data : [...rhis.boxList,...res.data.data],
						this.pages = res.data.last_page
					}
				})
			},
			//获取价格区间
			getPriceList () {
				this.$api.priceRange().then(res=>{
					if (res.code === 1) {
						this.priceList = res.data.range
					}
				})
			}
		},
		onLoad() {
			this.getPriceList()
			this.getBoxListByCategory()
		},
		onPullDownRefresh() {
			this.page = 1
			this.getBoxListByCategory()
		},
		onReachBottom() {
			if (this.page < this.pages) {
				this.page++
				this.getBoxListByCategory()
			}
		},
	}
</script>

<style lang="scss">
	.screen {
		position: fixed;
		bottom: 0;
		right: 0;
		width: 0;
		overflow: hidden;
		height: calc(100vh - 88px);
		background: #FFFFFF;
		z-index: 2021;
		transition: all 0.2s ease;
		.screen_box {
			padding: 30rpx 20rpx 0 30rpx;
			.screen_name {
				font-size: 28rpx;
				margin-bottom: 24rpx;
			}
			.screen_head {
				.screen_ipt {
					width: 168rpx;
					height: 64rpx;
					padding: 0 30rpx;
					background: #F5F5F5;
					border-radius: 32rpx;
					input {
						font-size: 26rpx;
						text-align: center;
					}
				}
				.screen_head_line {
					width: 27rpx;
					height: 3rpx;
					margin: 0 20rpx;
					background: #999999;
				}
			}
			.screen_ul {
				flex-wrap: wrap;
				.screen_li {
					width: 166rpx;
					height: 64rpx;
					margin: 24rpx 24rpx 0 0;
					font-size: 26rpx;
					background: #F5F5F5;
					border-radius: 32rpx;
					&:nth-child(3n) {
						margin-right: 0;
					}
				}
				.active {
					color: #FFFFFF;
					background: #F6AF32;
				}
			}
		}
		.screen_footer {
			width: 100%;
			bottom: 0;
			
			position: absolute;
			.screen_footer_li {
				width: 260rpx;
				height: 68rpx;
				color: #666666;
				font-size: 32rpx;
				background: #F5F5F5;
				&:last-child {
					color: #FFFFFF;
					background: #F6AF32;
				}
			}
		}
	}
	.marking {
		position: fixed;
		bottom: 0;
		right: 0;
		width: 100%;
		height: calc(100vh - 88px);
		background: rgba(0,0,0,0.5);
	}
	.more_head {
		top: 88rpx;
		left: 0;
		width: 100%;
		height: 88rpx;
		z-index: 20;
		position: sticky;
		background: #FFFFFF;
		margin-bottom: 30rpx;
		padding: 0 30rpx;
		.more_head_li {
			text {
				font-size: 30rpx;
			}
			image {
				width: 22rpx;
				height: 22rpx;
				transition: all 0.2s ease;
				margin-left: 10rpx;
			}
			.handstand {
				transform: rotate(180deg);
			}
		}
		.active {
			text {
				color: #F6AF32;
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
