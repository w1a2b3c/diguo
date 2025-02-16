<template>
	<view class="me">
		<!-- S 头部 -->
		<view class="me_head flexs" @click="goSet">
			<view class="txbj">
			<image :src="info.avatar" mode="aspectFill"></image>
			</view>
			<text>{{ info.nickname }}</text>
		</view>
		<!-- E 头部-->
		<!-- 中间 -->
		<view class="me_main">
			<view class="me_main_head">
				<view class="me_main_head_nav flexs">
					<view class="me_main_head_nav_li center" v-for="(item,index) in topList" :key="index" @click="changeTop(index)">
						<image :src="item.image" mode=""></image>
						<text>{{ item.name }}</text>
					</view>
				</view>
				<view class="me_main_box">
					<view class="me_main_box_order flex">
						<view class="me_main_box_order_txt">我的订单</view>
						<view class="me_main_box_order_go flexs" @click="order(0)">
							<text>全部订单</text>
							<image src="/static/image/publice/jinruer@2x.png" mode=""></image>
						</view>
					</view>
					<view class="me_main_head_ul flex">
						<view class="me_main_head_li center" v-for="(item,index) in orderList" :key="index" @click="order(index)">
							<image :src="item.image" mode=""></image>
							<text>{{ item.name }}</text>
						</view>
					</view>
				</view>
			</view>
			<view class="me_main_con">
				<view class="me_main_con_head">钱包</view>
				<view class="me_main_con_gold flex">
					<view class="me_main_con_gold_l flexs" @click="goWallet">
						<text>金币：</text>
						<text>{{ info.coin || 0 }}</text>
					</view>
					<view class="me_main_con_gold_btn center" @click="recharge">充值</view>
				</view>
			</view>
			<view class="me_main_footer">
				<view class="me_main_footer_li flex" v-for="(item,index) in footerList" :key="index" @click="changeFooter(item)">
					<view class="me_main_footer_img flexs">
						<image :src="item.image" mode=""></image>
						<text>{{ item.name }}</text>
					</view>
					<view class="me_main_footer_li_go">
						<image src="/static/image/publice/jinruer@2x.png" mode=""></image>
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
				topList:[
					{
						image:'/static/image/me/hegui@2x.png',
						name:'我的盒柜',
						url:''
					},
					{
						image:'/static/image/me/jilu.png',
						name:'开箱记录',
						url:''
					}
				],
				orderList:[
					{
						image:'/static/image/me/daifahuo@2x.png',
						name:'待发货'
					},
					{
						image:'/static/image/me/yifahuo@2x.png',
						name:'已发货'
					},
					{
						image:'/static/image/me/succed.png',
						name:'已完成'
					}
				],
				footerList:[
					{
						image:'/static/image/me/yue@2x.png',
						name:'我的余额',
						url:'/pages/me/balance'
					},
					// {
					// 	image:'/static/image/me/shoucang@2x.png',
					// 	name:'提现',
					// 	url:'/pages/me/deposit'
					// },
					{
						image:'/static/image/me/shoucang@2x.png',
						name:'我的收藏',
						url:'/pages/me/coll'
					},
					{
						image:'/static/image/me/fenxiang@2x.png',
						name:'分享赚钱',
						url:'/pagesA/pages/invite'
					},
					{
						image:'/static/image/me/dizhi@2x.png',
						name:'收货地址',
						url:'/pages/me/address'
					},
					{
						image:'/static/image/me/kf1.png',
						name:'联系客服',
						url:'/pagesB/pages/server'
					},
					{
						image:'/static/image/me/shezhi@2x.png',
						name:'设置',
						url:'/pages/me/set'
					},
				],
				info:{},//个人信息
			};
		},
		methods:{
			goSet () {
				uni.navigateTo({url:'/pages/me/set'})
			},
			//去我的钱包
			goWallet () {
				uni.navigateTo({url:'/pages/me/wallet'})
			},
			//去充值
			recharge () {
				uni.navigateTo({url:'/pages/me/recharge'})
			},
			//头部
			changeTop (index) {
				index == 0 ? uni.navigateTo({url:'/pages/me/myBox'}) : uni.navigateTo({url:'/pages/me/record'})
			},
			//去我的订单
			order (index) {
				uni.navigateTo({url:'/pages/me/order?type=' + index})
			},
			changeFooter (item) {
				uni.navigateTo({url:item.url})
			},
			//获取个人信息
			getInfo () {
				this.$api.userinfo().then(res=>{
					if (res.code === 1) {
						this.info = res.data
					}
				}).catch(res=>{
					console.log(res);
				})
			}
		},
		onShow() {
			this.getInfo()
		}
	}
</script>

<style lang="scss">
.me_head {
	height: 394rpx;
	padding-left: 47rpx;
	background: url(../../static/image/tabbar/xxbj.jpg);
	image {
		width: 100rpx;
		height: 100rpx;
		margin-right: 26rpx;
		border-radius: 50%;
		position: relative;
		left: 40rpx;
		top:40rpx;
	}
	text {
		color: #FFFFFF;
		font-size: 36rpx;
	}
}
.me_head .txbj{
	width: 180rpx;
	height: 180rpx;
	background: url(../../static/image/tabbar/txbj.png);
	background-size: 100%;
	z-index: 99999;
}
.me_main {
	padding: 0 30rpx;
	margin-top: -74rpx;
	padding-bottom: 48rpx;
	.me_main_head {
		padding: 0 24rpx;
		margin-bottom: 20rpx;
		background: #FFFFFF;
		box-shadow: 0rpx 0rpx 10rpx 0rpx rgba(51, 51, 51, 0.1);
		border-radius: 20rpx;
		.me_main_head_nav {
			border-bottom: 2rpx solid #F7F7F7;
			.me_main_head_nav_li {
				width: 50%;
				height: 86rpx;
				image {
					width: 44rpx;
					height: 44rpx;
					margin-right: 10rpx;
				}
				text {
					color: #000000;
					font-size: 28rpx;
					font-weight: bold;
				}
			}
		}
		.me_main_box {
			padding: 0 14rpx 30rpx 14rpx;
			.me_main_box_order {
				padding: 30rpx 0 35rpx 0;
			}
			.me_main_box_order_txt {
				color: #010101;
				font-size: 28rpx;
				font-weight: bold;
			}
			.me_main_box_order_go {
				image {
					width: 22rpx;
					height: 22rpx;
					margin-left: 10rpx;
				}
				text {
					color: #999999;
					font-size: 22rpx;
				}
			}
			.me_main_head_ul {
				.me_main_head_li {
					flex-direction: column;
					image {
						width: 44rpx;
						height: 44rpx;
						margin-bottom: 10rpx;
					}
					text {
						color: #000000;
						font-size: 22rpx;
					}
				}
			}
		}
	}
	.me_main_con {
		background: #FFFFFF;
		padding: 0 30rpx;
		margin-bottom: 2rpx;
		box-shadow: 0rpx 0rpx 6rpx 0rpx rgba(51, 51, 51, 0.1);
		border-radius: 20rpx;
		.me_main_con_head {
			padding: 30rpx 0;
			color: #010101;
			font-size: 28rpx;
			font-weight: bold;
		}
		.me_main_con_gold {
			padding: 22rpx 0 26rpx 0;
			border-top: 2rpx solid #F7F7F7;
			.me_main_con_gold_l {
				text {
					color: #010101;
					font-size: 28rpx;
					&:last-child {
						font-weight: bold;
					}
				}
			}
			.me_main_con_gold_btn {
				width: 100rpx;
				height: 44rpx;
				color: #FFFFFF;
				background: #FA9F00;
				border-radius: 22rpx;
			}
		}
	}
	.me_main_footer_li {
		padding: 30rpx;
		height: 88rpx;
		margin-bottom: 2rpx;
		background: #FFFFFF;
		border-radius: 20rpx;
		.me_main_footer_img {
			image {
				width: 44rpx;
				height: 44rpx;
				margin-right: 20rpx;
			}
			text {
				color: #000000;
				font-size: 28rpx;
			}
		}
		.me_main_footer_li_go {
			width: 22rpx;
			height: 22rpx;
		}
	}
}
</style>
