<template>
	<view class="deposit">
		<view class="deposit_head">
			<view class="deposit_head_name">提现金额</view>
			<view class="deposit_head_main">
				<view class="deposit_head_txt">提现比例1：1</view>
				<view class="deposit_head_ipt flexs">
					<text>￥</text>
					<input type="digit" placeholder="请输入" v-model="amount" maxlength="9" placeholder-style="color:#999999" />
				</view>
			</view>
		</view>
		<view class="deposit_main">
			<view class="deposit_main_name">选择提现方式</view>
			<view class="deposit_main_ul">
				<view class="deposit_main_li flex" v-for="(item,index) in payList" :key="index" @click="changePay(item.type)">
					<view class="deposit_main_img flexs" @click.stop="bindAccount(index)">
						<image :src="item.image" mode=""></image>
						<text>{{ item.name }}{{ item.account ? '('+ item.account +')' : ' (请绑定)' }}</text>
					</view>
					<view class="deposit_main_li_select">
						<image :src="item.type == payIndex ? '/static/image/publice/xuanzhong1@2x.png' : '/static/image/publice/weixuanzhong@2x.png'" mode=""></image>
					</view>
				</view>
			</view>
			<view class="deposit_main_txt"><u-parse :content="message" ></u-parse></view>
		</view>
		<button class="deposit_btn" hover-class="hover-view" @click="submit">提交</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				message:'',
				amount:'',//提现金额
				payIndex:0,//支付方式
				payList:[
					{
						name:'微信提现',
						type:'0',
						account:'',
						image:'/static/image/me/weixin@2x.png'
					},
					{
						name:'支付宝提现',
						type:'1',
						account:'',
						image:'/static/image/me/zhifubao@2x.png'
					}
				],//支付列表
			};
		},
		methods:{
			submit () {
        // return uni.showToast({title:'不支持体现',icon:'none'})
				if (!this.amount) return uni.showToast({title:'请输入金额',icon:'none'})
				if (!this.payList[this.payIndex].account) return uni.showToast({title:'请绑定' + (this.payIndex == 0 ?'微信' : '支付宝') +'账号',icon:'none'})
				if (this.amount == 0) return
				this.$api.withdrawal({amount:this.amount,type:this.payIndex == 0 ? 'wechat' : 'alipay'}).then(res=>{
					if (res.code === 1) {
						uni.showToast({title:res.msg})
						this.amount = ''
					}
				})
			},
			//绑定账号
			bindAccount (index) {
				uni.navigateTo({url: '/pages/me/WeChat?type=' + index})
			},
			changePay (index) {
				this.payIndex = index
			},
			//huoqu
			getAccount () {
				this.$api.getWithdrawalSetting().then(res=>{
					if (res.code === 1) {
						this.payList[1].account = res.data.alipay.account
						this.payList[0].account = res.data.wechat.account
					}
				})
			},
			getMessage () {
				this.$api.agreement({name:'withdrawal_instruction'}).then(res=>{
					if (res.code === 1) {
						this.message = res.data.content
					}
				})
			}
		},
		onShow() {
			this.getAccount()
		},
		onLoad() {
			this.getMessage()
		}
	}
</script>

<style lang="scss">
.deposit {
	padding: 0 35rpx 0 25rpx;
	.deposit_head {
		.deposit_head_name {
			font-size: 32rpx;
			padding: 30rpx 0 20rpx 20rpx;
		}
		.deposit_head_main {
			background: #FFFFFF;
			padding: 30rpx 30rpx 58rpx 20rpx;
			.deposit_head_txt {
				font-size: 28rpx;
				margin-bottom: 30rpx;
			}
			.deposit_head_ipt {
				padding: 20rpx 0;
				border-bottom: 2rpx solid #EBEBEB;
				text {
					font-size: 36rpx;
					margin-right: 20rpx;
				}
				input {
					color: #F6AF32;
					font-size: 36rpx;
				}
			}
		}
	}
}
.deposit_main {
	.deposit_main_name {
		font-size: 32rpx;
		padding: 50rpx 0 30rpx 20rpx;
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
		padding: 20rpx 0;
	}
}
.deposit_btn {
	height: 98rpx;
	color: #333333;
	font-size: 30rpx;
	margin-top: 150rpx;
	background: #FFFFFF;
	box-shadow: 0rpx 0rpx 12rpx 0rpx rgba(220, 220, 220, 0.2);
	border-radius: 20rpx;
}
</style>
