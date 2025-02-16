<template>
	<view class="recharge">
		<view class="recharge_head">
			<view class="recharge_head_txt flex">
				<text>选择充值金额</text>
				<text>{{ tips }}</text>
			</view>
			<view class="recharge_head_ul flexs">
				<view class="recharge_head_li center" :class="{active:index == i}" @click="changeI(index)" v-for="(item,index) in rechargeList" :key="index">
					<text>￥</text>
					<text>{{ item.coin }}</text>
				</view>
			</view>
		</view>
		<view class="deposit_main">
			<view class="deposit_main_name">选择支付方式，支付成功以后记得在支付界面点完成以防掉单补单！</view>
			<view class="deposit_main_ul">
				<view class="deposit_main_li flex" v-for="(item,index) in payList" :key="index" @click="changePay(item.type)">
					<view class="deposit_main_img flexs">
						<image :src="item.image" mode=""></image>
						<text>{{ item.name }}</text>
					</view>
					<view class="deposit_main_li_select">
						<image :src="item.type == payIndex ? '/static/image/publice/xuanzhong1@2x.png' : '/static/image/publice/weixuanzhong@2x.png'" mode=""></image>
					</view>
				</view>
			</view>
			<view class="deposit_main_txt"><u-parse :content="message"></u-parse></view>
		</view>
		<button class="recharge_btn" hover-class="hover-view" @click="submitPay">支付</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				i:0,//选择金额
				message:'',
				tips:'',
				payIndex:1,//支付方式
				rechargeList:[],//
				payList:[
					 {
						name:'微信支付',
						type:'0',
						image:'/static/image/me/weixin@2x.png'
					},
					{
						name:'支付宝支付',
						type:'1',
						image:'/static/image/me/zhifubao@2x.png'
					},
				
				/*	{
						name:'普通支付',
						type:'2',
						image:'/static/image/me/zhifubao@2x_no.png'
					},
					
					*/
					
					
					
				],//支付列表
			};
		},
		methods:{
			//提交支付
			submitPay () {
				this.$api.createRechargeOrder({amount:this.rechargeList[this.i].coin}).then(res=>{
					if (res.code === 1) {
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
													uni.redirectTo({url:'/pages/me/wallet'})
												},800)
												
											}
									});
								} else if (res.code === 403) {
									
								}
							})
						} else {
							
							var fs='wechat';
							if(this.payIndex==0){
									fs='wechat';
							}
							else if (this.payIndex==1) {
									fs='alipay';
								
							}
							else if (this.payIndex==2) 
							{
								
								fs='eepay';	
							}
							
							else {
									fs='wechat';
								
							}
							
							
							location.href = res.data[fs]+'?token=' + uni.getStorageSync('token') //res.data[this.payIndex == 0 ? 'wechat' : 'alipay'] + '?token=' + uni.getStorageSync('token') 
						}
						 
					}
				})
			},
			//选择金额
			changeI (index) {
				this.i = index
			},
			//切换支付方式
			changePay (index) {
				this.payIndex = index
			},
			
			//获取充值列表
			getRechargeList () {
				this.$api.rechargeList().then(res=>{
					if (res.code === 1) {
						this.tips = res.data.tips
						this.message = res.data.notice
						this.rechargeList = res.data.list
					}
				})
			}
		},
		onLoad() {
			this.getRechargeList()
		}
	}
</script>

<style lang="scss">
.recharge {
	min-height: calc(100vh - 44px);
	position: relative;
	padding: 0 30rpx;
	.recharge_head {
		.recharge_head_txt {
			padding: 50rpx 0 30rpx 0;
			text {
				&:first-child {
					font-size: 32rpx;
					font-weight: bold;
				}
				&:last-child {
					color: #999999;
				}
			}
		}
	}
	.recharge_head_ul {
		flex-wrap: wrap;
		.recharge_head_li {
			margin: 0 24rpx 20rpx 0;
			width: 214rpx;
			height: 160rpx;
			background: #FFFFFF;
			border: 2rpx solid #EBEBEB;
			border-radius: 10rpx;
			&:nth-child(3n) {
				margin-right: 0;
			}
			text {
				color: #F8860D;
				font-size: 30rpx;
				&:last-child {
					font-size: 44rpx;
				}
			}
		}
		.active {
			border: 2rpx solid #F8860D;
		}
	}
}
.deposit_main {
	padding-bottom: 30rpx;
	.deposit_main_name {
		font-size: 32rpx;
		font-weight: bold;
		padding: 40rpx 0 30rpx 20rpx;
	}
	.deposit_main_ul {
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
	.deposit_main_txt {
		padding: 30rpx 0;
	}
}
.recharge_btn {
	/*position: absolute;
	left: 50%;
	transform: translateX(-50%);
	*/
	width: 690rpx;
	bottom: 34rpx;
	
	color: #fff;
	
	height: 88rpx;
	background: #f6af32;
	box-shadow: 0rpx 0rpx 12rpx 0rpx rgba(220, 220, 220, 0.2);
	border-radius: 8rpx;
}
</style>
