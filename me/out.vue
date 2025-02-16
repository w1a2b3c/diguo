<template>
	<view class="deposit">
		<view class="deposit_head">
			<view class="deposit_head_name">转出金额</view>
			<view class="deposit_head_main">
				<view class="deposit_head_txt">输入金额</view>
				<view class="deposit_head_ipt flexs">
					<text>￥</text>
					<input type="digit" v-model="amount" placeholder="请输入" placeholder-style="color:#999999" />
				</view>
			</view>
		</view>
		<view class="deposit_main">
			<view class="deposit_main_txt"><u-parse :content="message"></u-parse></view>
		</view>
		<button class="deposit_btn" hover-class="hover-view" @click="submit">提交</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				amount:'',
				message:'',
			};
		},
		methods:{
			submit () {
				if (!this.amount) return uni.showToast({title:'请输入金额',icon:'none'})
				this.$api.moneyToCoin({amount:this.amount}).then(res=>{
					if (res.code === 1) {
						this.amount = ''
						uni.showToast({title:res.msg})
					}
				})
			},
			getMessage () {
				this.$api.agreement({name:'balance_to_wallet_instruction'}).then(res=>{
					if (res.code === 1) {
						this.message = res.data.content
					}
				})
			}
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
	.deposit_main_txt {
		padding: 20rpx 0;
	}
}
.deposit_btn {
	height: 98rpx;
	color: #333333;
	font-size: 30rpx;
	margin-top: 480rpx;
	background: #FFFFFF;
	box-shadow: 0rpx 0rpx 12rpx 0rpx rgba(220, 220, 220, 0.2);
	border-radius: 20rpx;
}
</style>
