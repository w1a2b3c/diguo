<template>
	<view class="apply">
		<view class="apply_head">
			<view class="apply_head_name">收货人信息</view>
			<view class="apply_head_ipt flex">
				<view class="apply_head_address">
					<view class="address_have_li_title">{{ details.province }} {{ details.city }} {{ details.area }}</view>
					<view class="address_have_li_txt">{{ details.detail }}</view>
					<view class="address_have_li_name flexs">
						<text>{{ details.username }}</text>
						<text>{{ details.mobile }}</text>
					</view>
				</view>
			</view>
		</view>
		<view class="apply_main">
			<view class="apply_main_name">订单信息</view>
			<view class="apply_main_ul">
				<view class="apply_main_li flexs">
					<view class="apply_main_li_img">
						<image :src="details.goods_image" mode="aspectFill"></image>
					</view>
					<view class="apply_main_li_main flex">
						<view class="apply_main_li_name">{{ details.goods_name }}</view>
						<view class="apply_main_li_time">开箱日期：{{ details.box_open_time }}</view>
					</view>
				</view>
			</view>
		</view>
		<view class="apply_order">
			<view class="apply_order_li flex">
				<text>付款金额</text>
				<text class="red">{{ details.pay_amount }}</text>
			</view>
			<view class="apply_order_li flex">
				<text>支付方式</text>
				<text>{{ details.pay_method }}</text>
			</view>
		</view>
		<view class="apply_order apply_order_number">
			<view class="apply_order_li flex">
				<text>订单号：{{  details.delivery_order_no}}</text>
			</view>
			<view class="apply_order_li flex">
				<text>下单时间：{{ details.delivery_apply_time }}</text>
			</view>
			<view class="apply_order_li flex" v-if="details.status == 'unreceived' || details.status == 'finished'">
				<text>发货时间：{{ details.delivery_time }}</text>
			</view>
			<view class="apply_order_li flex" v-if="details.status == 'unreceived' || details.status == 'finished'">
				<text>物流号：{{ details.delivery_number }}</text>
				<!-- <text class="logistics" @click="lookLogistics" v-if="details.status == 'unreceived'">查看物流</text> -->
			</view>
			<view class="apply_order_li flex" v-if="details.status == 'unreceived' || details.status == 'finished'">
				<text>物流名称：{{ details.post_name }}</text>
				<!-- <text class="logistics" @click="lookLogistics" v-if="details.status == 'unreceived'">查看物流</text> -->
			</view>
			<view class="apply_order_li flex" v-if="details.status == 'finished'">
				<text>收货时间：{{ details.receive_time }}</text>
			</view>
		</view>
		<button class="apply_btn" hover-class="hover-view" v-if="details.status == 'unreceived'" @click="affirm">确认收货</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				id:'',//订单ID
				details:{},//详情
			};
		},
		methods:{
			//查看物流
			lookLogistics () {
				uni.navigateTo({url:'/pages/me/logistics'})
			},
			//获取订单详情
			getDetail () {
				this.$api.deliveryOrderDetail({delivery_order_id:this.id}).then(res=>{
					if (res.code === 1) {
						this.details = res.data
					}
				})
			},
			//确认收货
			affirm () {
				uni.showModal({
					content: '是否确认收货？',
					success:  (res) => {
						if (res.confirm) {
							this.$api.confirmReceipt({delivery_order_id:this.id}).then(res=>{
								if (res.code === 1) {
									uni.showToast({title:res.msg})
									setTimeout(()=>{
										uni.navigateBack()
									},800)
								}
							})
						}
					}
				});
				
			}
		},
		onLoad ({id}) {
			this.id = id
			this.getDetail()
		}
	}
</script>

<style lang="scss">
.apply {
	padding: 0 30rpx 38rpx 30rpx;
	.apply_head_name {
		padding: 30rpx 0;
		font-size: 28rpx;
		font-weight: bold;
	}
	.apply_head_ipt {
		padding: 30rpx;
		border-radius: 10rpx;
		background: #FFFFFF;
		input {
			font-size: 28rpx;
		}
	}
	.apply_head_address {
		.address_have_li_title {
			color: #666666;
			font-size: 28rpx;
		}
		.address_have_li_txt {
			font-size: 26rpx;
			margin: 20rpx 0;
		}
		.address_have_li_name {
			text {
				color: #999999;
				font-size: 30rpx;
				&:last-child {
					font-size: 28rpx;
					margin-left: 20rpx;
				}
			}
		}
	}
}
.apply_main_name {
	font-size: 28rpx;
	font-weight: bold;
	padding: 30rpx 0 20rpx 0;
	
}
.apply_main_li {
		padding: 30rpx;
		background: #FFFFFF;
		border-radius: 20rpx;
		.apply_main_li_img {
			width: 168rpx;
			height: 168rpx;
			image {
				border-radius: 10rpx;
			}
			margin-right: 20rpx;
		}
		.apply_main_li_main {
			height: 168rpx;
			align-items: flex-start;
			flex-direction: column;
			justify-content: space-around;
		}
		.apply_main_li_name {
			font-size: 28rpx;
			line-height: 36rpx;
		}
		.apply_main_li_time {
			color: #999999;
		}
	}
	.apply_order {
		padding: 0 30rpx 0 20rpx ;
		border-radius: 10rpx;
		background: #FFFFFF;
		margin-top: 20rpx;
		.apply_order_li {
			padding: 30rpx 0;
			text {
				font-size: 26rpx;
				&:last-child {
					color: #999999;
				}
			}
			.red {
				color: #CF271B !important;
			}
		}
	}
	.apply_order_number {
		padding: 30rpx 20rpx;
		.apply_order_li {
			padding: 0;
			margin-bottom: 30rpx;
			&:last-child {
				margin-bottom: 0;
			}
			.logistics {
				width: 130rpx;
				height: 44rpx;
				text-align: center;
				line-height: 44rpx;
				color: #FFFFFF !important;
				font-size: 24rpx;
				background: #F6AF32;
				border-radius: 22rpx;
			}
		}
	}
	.apply_btn {
		color: #333333;
		font-size: 30rpx;
		font-weight: bold;
		height: 98rpx;
		margin-top: 45rpx;
		background: #FFFFFF;
		box-shadow: 0rpx 0rpx 121rpx 0rpx rgba(63, 52, 2, 0.12);
		border-radius: 10rpx;
	}
</style>
