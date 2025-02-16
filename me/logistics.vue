<template>
	<view class="logistics">
		<view class="logistics_ul">
			<view class="logistics_li" v-for="(item,index) in 10" :key="index">
				<view class="logistics_li_left">
					<view class="logistics_li_left_dot" v-if="index == 0">
						<image src="/static/image/me/daoda@2x.png" mode=""></image>
					</view>
					<view class="logistics_li_left_xiao" v-else>
						<image src="/static/image/me/wuliu@2x.png" mode=""></image>
					</view>
					
				</view>
				<view class="logistics_li_main">
					<view class="logistics_li_name">订单已装车完成，下一站：郑州市，操作人：杨爱华</view>
					<view class="logistics_li_time">2018-10-12</view>
				</view>
			</view>
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

<style lang="scss">
	.logistics {
		padding: 20rpx 30rpx 30rpx;
	}
.logistics_ul {
	overflow: hidden;
	position: relative;
	border-radius: 20rpx;
	padding: 38rpx 40rpx 0 50rpx;
	background: #FFFFFF;
	&::after {
		position: absolute;
		left: 50rpx;
		content: '';
		width: 2rpx;
		height: 97%;
		bottom: 0;
		background: #CCCCCC;
		border-radius: 1rpx;
	}
	.logistics_li {
		.logistics_li_left {
			margin-right: 40rpx;
			.logistics_li_left_dot {
				width: 44rpx;
				height: 44rpx;
				position: relative;
				image {
					top: 11rpx;
					left: -22rpx;
					z-index: 20;
					position: absolute;
				}
			}
			.logistics_li_left_xiao {
				width: 22rpx;
				height: 22rpx;
				position: relative;
				image {
					top: 11rpx;
					left: -11rpx;
					position: absolute;
				}
			}
			
		}
		margin-bottom: 60rpx;
		display: flex;
		&:first-child {
			.logistics_li_name {
				color: #333333;
				font-size: 24rpx;
			}
			
		}
		.logistics_li_name {
			color: #999999;
		}
		.logistics_li_time {
			color: #999999;
			font-size: 22rpx;
			margin-top: 10rpx;
		}
	}
}
</style>
