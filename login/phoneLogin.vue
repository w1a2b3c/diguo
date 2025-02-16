<template>
	<view class="register">
		<view class="register_head">
			<text>手机快捷登录</text>
			<text>未注册过的手机号将自动创建账号</text>
		</view>
		<view class="register_ul">
			<view class="register_li flex">
				<view class="register_ipt">
					<input type="number" maxlength="11" placeholder="请输入手机号" placeholder-style="color:#999999" />
				</view>
			</view>
			<view class="register_li flex">
				<view class="register_ipt">
					<input type="password" maxlength="12" placeholder="请输入验证码" placeholder-style="color:#999999" />
				</view>
				<view class="code center">获取验证码</view>
			</view>
		</view>
		<button class="register_btn" hover-class="hover-view">登录</button>
		<view class="register_footer center">
			<text>密码登录</text>
			<text></text>
			<text>注册账号</text>
		</view>
		<view class="register_fast center">
			<text>快捷登录</text>
			<image src="/static/logo.png" mode=""></image>
		</view>
		<view class="register_consent center">
			<image src="/static/logo.png" mode=""></image>
			<text>登录即表明同意</text>
			<text class="blue">《用户协议》</text>
			<text>和</text>
			<text class="blue">《隐私政策》</text>
		</view>
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
<style>
	page {
		background: #FFFFFF;
	}
</style>
<style lang="scss">
.register {
	min-height: 100vh;
	padding: 0 42rpx;
	position: relative;
	.register_head {
		display: flex;
		flex-direction: column;
		padding: 30rpx 0 40rpx 0;
		text {
			&:first-child {
				font-size: 40rpx;
				font-weight: bold;
			}
			&:last-child {
				color: #999999;
				font-size: 26rpx;
				margin-top: 20rpx;
			}
		}
	}
	.register_li {
		padding: 40rpx 0;
		border-bottom: 2rpx solid #DDDDDD;
		.register_ipt {
			flex: 1;
			input {
				width: 100%;
				font-size: 30rpx;
			}
		}
		.code {
			width: 150rpx;
			color: #4EABFC;
			font-size: 28rpx;
		}
	}
}
.register_btn {
	height: 80rpx;
	margin-top: 60rpx;
	background: #F6AF32;
	border-radius: 40rpx;
}
.register_code {
	color: #4EABFC;
	font-size: 28rpx;
	text-align: right;
	margin: 30rpx 0 50rpx 0;
}
.register_footer {
	margin: 50rpx 0 125rpx 0;
	text {
		color: #4EABFC;
		font-size: 26rpx;
		&:nth-child(2) {
			width: 1px;
			height: 25rpx;
			margin: 0 35rpx;
			background: #4EABFC;
		}
		
	}
}
.register_fast {
	flex-direction: column;
	text {
		color: #999999;
		margin-bottom: 40rpx;
	}
	image {
		width: 71rpx;
		height: 71rpx;
	}
}
.register_consent {
	width: 100%;
	position: absolute;
	left: 50%;
	bottom: 70rpx;
	transform: translateX(-50%);
	image {
		width: 30rpx;
		height: 30rpx;
		flex-shrink: 0;
		margin-right: 10rpx;
	}
	.blue {
		color: #4EABFC;
	}
}
</style>
