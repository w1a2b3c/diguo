<template>
	<view class="balance">
		<view class="balance_head">
			<view class="balance_head_bj center">
				<text>{{ balancePrice }}</text>
				<text>余额:(金币）</text>
			</view>
			<view class="balance_head_ul flex" style="justify-content: center;">
				<!-- <view class="balance_head_li center" v-for="(item,index) in 2" :key='index' @click="changeBalance(index)"> {{ index == 0 ? '转到钱包' : '提现' }}</view> -->
				<view class="balance_head_li center" @click="changeBalance(0)">转到钱包</view>
			</view>
		</view>
		<view class="balance_main">
			<view class="balance_main_head flexs">
				<view class="balance_main_li center" v-for="(item,index) in 2" :key="index" :class="{active:index == i}" @click="changeI(index)">{{ index == 0 ? '收入' : '支出' }}</view>
			</view>
			<view class="balance_main_list">
				<view class="balance_main_list_li flex" v-for="(item,index) in balanceList" :key="index">
					<view class="balance_main_list_li_con">
						<view class="balance_main_list_li_name">{{ item.type }}</view>
						<view class="balance_main_list_li_time">时间：{{ item.create_time }}</view>
					</view>
					<view class="balance_main_list_li_price" :class="{blue:i == 1}">{{ item.money }}</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				i:0,//
				page:1,//页码
				pages:null,//总页数
				balanceList:[],//
				balancePrice:'',//余额详情
			};
		},
		methods:{
			//选择哪个
			changeBalance (index) {
				uni.navigateTo({url:index == 0 ? '/pages/me/out' : '/pages/me/deposit'})
			},
			//切换
			changeI (index) {
				this.i = index
				this.page = 1
				this.balanceList = []
				this.getBalanceList()
			},
			//
			getBalanceList () {
				this.$api.myBalance({page:this.page,type:this.i == 0 ? 'in' : 'out',msg:'数据加载中'}).then(res=>{
					uni.stopPullDownRefresh()
					if (res.code === 1) {
						this.balancePrice = res.data.balance
						this.pages = res.data.record.last_page
						this.balanceList = this.page == 1 ? res.data.record.data : [...this.balanceList,...res.data.record.data]
					}
				})
			}
		},
		onLoad() {
			this.getBalanceList()
		},
		onPullDownRefresh() {
			this.page = 1
			this.balanceList = []
			this.getBalanceList()
		},
		onReachBottom() {
			if (this.page < this.pages) {
				this.page++
				this.getBalanceList()
			}
		}
	}
</script>

<style lang="scss">
.balance {
	padding: 0 30rpx;
	.balance_head {
		margin-bottom: 60rpx;
		padding: 30rpx 0;
		.balance_head_bj {
			height: 220rpx;
			margin-bottom: 30rpx;
			background: url(../../static/image/me/yuebeijing@2x.png) no-repeat;
			background-size: cover;
			flex-direction: column;
			text {
				color: #FFFFFF;
				&:first-child {
					font-size: 60rpx;
					font-weight: bold;
				}
				&:last-child {
					font-size: 28rpx;
					margin-top: 30rpx;
				}
			}
		}
		.balance_head_li {
			width: 330rpx;
			height: 78rpx;
			color: #FFFFFF;
			font-size: 30rpx;
			border-radius: 39rpx;
			&:first-child {
				background: #FA7E48;
			}
			&:last-child {
				background: #F6AF32;
			}
		}
	}
}
.balance_main_head {
	background: #FFFFFF;
	border-radius: 10rpx 10rpx 0rpx 0rpx;
	border-bottom: 2rpx solid #FAFAFA;
	.balance_main_li {
		width: 50%;
		height: 80rpx;
		font-size: 30rpx;
	}
	.active {
		color: #F6AF32;
		position: relative;
		font-weight: bold;
		&::after {
			position: absolute;
			content: '';
			bottom: 0;
			left: 50%;
			transform: translateX(-50%);
			width: 40rpx;
			height: 4rpx;
			background: #F6AF32;
			border-radius: 2rpx;
		}
	}
}
.balance_main_list {
	background: #FFFFFF;
	border-radius: 0rpx 0rpx 10rpx 10rpx;
	.balance_main_list_li {
		padding: 20rpx 30rpx;
		border-bottom: 2rpx solid #FAFAFA;
		.balance_main_list_li_name {
			color: #666666;
			font-size: 26rpx;
			margin-bottom: 20rpx;
		}
		.balance_main_list_li_time {
			color: #999999;
		}
		.balance_main_list_li_price {
			color: #F6AF32;
			font-size: 30rpx;
		}
		.blue {
			color: #327CF6;
		}
	}
}
</style>
