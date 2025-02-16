<template>
	<view class="apply">
		<view class="apply_head">
			<view class="apply_head_name">收货人信息</view>
			<view class="apply_head_ipt flex" @click="selectAddress">
				<input type="text" disabled v-if="!applyAddress.address_id" placeholder="填写收货地址" placeholder-style="color:#999999" />
				<view class="apply_head_address" v-else>
					<view class="address_have_li_title">{{ applyAddress.province }} {{ applyAddress.city }} {{ applyAddress.area }}</view>
					<view class="address_have_li_txt">{{ applyAddress.detail }}</view>
					<view class="address_have_li_name flexs">
						<text>{{ applyAddress.username }}</text>
						<text>{{ applyAddress.mobile }}</text>
					</view>
				</view>
				<image src="/static/image/publice/jinruer@2x.png" mode=""></image>
			</view>
		</view>
		<view class="apply_main">
			<view class="apply_main_name">订单信息</view>
			<view class="apply_main_ul">
				<view class="apply_main_li " v-for="(item,index) in applyList" :key="index">
					<view class="apply_main_li_box flexs">
						<view class="apply_main_li_img">
							<image :src="item.goods_image" mode="aspectFill"></image>
						</view>
						<view class="apply_main_li_main flex">
							<view class="apply_main_li_name">{{ item.goods_name }}</view>
							<view class="apply_main_li_time">开箱日期：{{ item.time }}</view>
						</view>
					</view>
					<view class="apply_order">
						<view class="apply_order_li flex">
							<text>付款金额</text>
							<text class="red">{{ item.pay_amount }}</text>
						</view>
						<view class="apply_order_li flex">
							<text>支付方式</text>
							<text>{{ item.pay_method }}</text>
						</view>
						<view class="apply_order_li flex">
							<text>运费</text>
							<text>{{ item.delivery_fee }}</text>
						</view>
					</view>
				</view>
			</view>
			<view class="deposit_main_ul">
				<view class="deposit_main_li flex" v-for="(item,index) in payList" :key="index" @click="changePay(item.type)">
					<view class="deposit_main_img flexs">
						<image :src="item.image" mode=""></image>
						<text>{{ item.name }}{{ item.account ? '('+ item.account +')' : '' }}</text>
					</view>
					<view class="deposit_main_li_select">
						<image :src="item.type == payIndex ? '/static/image/publice/xuanzhong1@2x.png' : '/static/image/publice/weixuanzhong@2x.png'" mode=""></image>
					</view>
				</view>
			</view>
		</view>
		<view class="apply_footer flex">
			<view class="apply_footer_total">合计：￥{{ totalPrice }}</view>
			<view class="apply_footer_btn center" @click="applySubmit">申请发货</view>
		</view>
		<!-- <button class="apply_btn" hover-class="hover-view" @click="applySubmit">申请发货</button> -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
				id:'',//回收ID
				flag:true,//
				totalPrice:'',
				applyList:[],//回收列表
				applyAddress:{},//收货地址
				payIndex:1,//支付方式
				payList:[
					/*{
						name:'微信支付',
						type:'0',
						account:'',
						image:'/static/image/me/weixin@2x.png'
					},*/
					{
						name:'支付宝支付',
						type:'1',
						account:'',
						image:'/static/image/me/zhifubao@2x.png'
					}
				],//支付列表
			};
		},
		methods:{
			//申请发货
			applySubmit () {
				if (!this.applyAddress.address_id) return uni.showToast({title:'请选择地址',icon:'none'})
				let ids = []
				this.applyList.forEach(item=>{
					ids.push(item.record_id)
				})
				if (!this.flag) return
				this.flag = false
				this.$api.applyDelivery({record_ids:ids.join(','),address_id:this.applyAddress.address_id}).then(res=>{
					if (res.code === 1) {
						
						if(res.data.delivery_fee > 0){    
							if (this.payIndex == 2) {
								this.$http.post(res.data.wechat).then(res=>{
									if (res.code === 1) {
										WeixinJSBridge.invoke('getBrandWCPayRequest', {
												"appId": res.data.appId,   //公众号ID，由商户传入    
												"timeStamp": res.data.timeStamp,   //时间戳，自1970年以来的秒数    
												"nonceStr": res.data.nonceStr,      //随机串    
												"package": res.data.package,
												"signType": res.data.signType,     //微信签名方式：    
												"paySign": res.data.paySign //微信签名
										},
										function(res) {
												if (res.err_msg == "get_brand_wcpay_request:ok") {
													uni.showToast({title:'支付成功'})
													setTimeout(()=>{
														uni.redirectTo({url:'/pages/me/order?type=0'})
													},800)
													
												}
										});
									} else if (res.code === 403) {
										
									}
								})
							} else {
								location.href = res.data[this.payIndex == 0 ? 'wechat' : 'alipay'] + '?token=' + uni.getStorageSync('token')
							}
							
						}else{
							// 跳转到支付页面
							uni.redirectTo({url:'/pages/me/order?type=0'})

						}
						
						
	
						// uni.removeStorageSync('applyAddress')
						// uni.showToast({title:res.msg})
						// setTimeout(()=>{
						// 	uni.navigateBack()
						// },800)
					} else {
						this.flag = true
					}
				})
			},
			//去选择地址
			selectAddress () {
				uni.navigateTo({url:'/pages/me/address?type=1'})
			},
			//获取回收信息
			getApply () {
				this.$api.checkDeliveryInfo({record_ids:this.id}).then(res=>{
					if (res.code === 1) {
						this.applyList = res.data.order_info
						this.totalPrice = res.data.total_delivery_fee
					}
				})
			},
			//获取默认地址
			getAddress () {
				this.$api.myAddress({is_default:1}).then(res=>{
					if (res.code === 1) {
						if (res.data.length != 0) {
							this.applyAddress = res.data[0]
						}
					}
				})
			},
			changePay (index) {
				this.payIndex = index
			},
		},
		onShow() {
			if (uni.getStorageSync('applyAddress')) {
				this.applyAddress = JSON.parse(uni.getStorageSync('applyAddress'))
			} else {
				this.getAddress()
			}
		},
		onLoad ({id}) {
			this.id = id
			this.getApply()
		}
	}
</script>

<style lang="scss">
	.deposit_main_ul {
		margin-top: 20rpx;
		background: #FFFFFF;
		.deposit_main_li {
			padding: 30rpx 20rpx;
			.deposit_main_img {
				image {
					width: 50rpx;
					height: 50rpx;
					margin-right: 20rpx;
				}
				text {
					font-size: 28rpx;
				}
			}
			.deposit_main_li_select {
				width: 32rpx;
				height: 32rpx;
			}
		}
	}
.apply {
	padding: 0 30rpx;
	padding-bottom: 60rpx;
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
		image {
			width: 22rpx;
			height: 22rpx;
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
.apply_main {
	padding-bottom: 98rpx;
}
.apply_main_li {
		margin-bottom: 20rpx;
		&:last-child {
			margin-bottom: 0;
		}
		.apply_main_li_box {
			padding: 30rpx;
			background: #FFFFFF;
		}
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
	.apply_btn {
		color: #333333;
		font-size: 30rpx;
		font-weight: bold;
		height: 98rpx;
		margin-top: 212rpx;
		background: #FFFFFF;
		box-shadow: 0rpx 0rpx 121rpx 0rpx rgba(63, 52, 2, 0.12);
		border-radius: 10rpx;
	}
	.apply_footer {
		width: 100%;
		left: 0;
		bottom: 0;
		
		height: 98rpx;
		
		position: fixed;
	
		padding-left: 30rpx;
		background: #FFFFFF;
		.apply_footer_total {
			font-size: 28rpx;
		}
		.apply_footer_btn {
			width: 280rpx;
			height: 100%;
			color: #FFFFFF;
			font-size: 28rpx;
				background: #F6AF32;
		}
	}
</style>
