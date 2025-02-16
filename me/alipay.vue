<template>
	<view class="alipay">
		<view class="alipay_ul">
			<view class="alipay_li flex">
				<text>真实姓名</text>
				<input type="text" placeholder="请填写真实姓名" placeholder-style="color:#999999" />
			</view>
			<view class="alipay_li flex">
				<text>支付宝账号</text>
				<input type="text" placeholder="请填写支付宝账号" placeholder-style="color:#999999" />
			</view>
		</view>
		<button hover-class="hover-view" class="alipay_btn">提交</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			};
		}
	}
</script>

<style lang="scss">
.alipay_li {
	height: 88rpx;
	padding:0 30rpx;
	margin-top: 2rpx;
	background: #FFFFFF;
	text {
		font-size: 28rpx;
	}
	input {
		text-align: right;
		font-size: 28rpx;
	}
}
.alipay_btn {
	width: 690rpx;
	height: 98rpx;
	color: #333333;
	margin: 666rpx auto 0;
	background: #FFFFFF;
	box-shadow: 0rpx 0rpx 12rpx 0rpx rgba(220, 220, 220, 0.2);
	border-radius: 20rpx;
}
</style>
