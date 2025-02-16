<template>
	<view class="shopList">
		<view class="shopList_head center">未成年人必须在家长监督下使用</view>
		<view class="shopList_ul flexs">
			<view class="shopList_li" v-for="(item,index) in boxDetail.goodslist" :key="index">
				<view class="shopList_li_con center">
					<view class="shopList_li_tag center" :class="{orange:item.tag == '稀有',red:item.tag == '至尊'}">{{ item.tag }}</view>
					<view class="shopList_li_img">
						<image :src="item.image" mode="aspectFill"></image>
					</view>
					<view class="shopList_li_price">
						<text>￥</text>
						<text>{{ item.price }}</text>
					</view>
				</view>
				<view class="shopList_li_name">{{ item.goods_name }}</view>
			</view>
		</view>
		<view class="shopList_footer flex" v-if="boxDetail.tags">
			<view class="shopList_footer_li" v-for="(item,index) in 3" :key="index">
				<text>{{ index == 0 ? '至尊款' : index == 1 ? '稀有款' : '普通款' }}</text>
				<text>概率{{ index == 0 ? boxDetail.tags.supreme : index == 1 ? boxDetail.tags.rare : boxDetail.tags.normal }}</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				type:null,//1试玩0详情
				box_id:'',//盲盒ID
				boxDetail:{},//盲盒详情
			};
		},
		methods:{
			//获取详情
			getDetails () {
				this.$api[this.type == 1 ? 'tryBoxDetail' : 'boxDetail' ]({box_id:this.box_id}).then(res=>{
					if (res.code === 1) {
						this.boxDetail = res.data.more
					}
				})
			}
		},
		onLoad({id,type}) {
			this.type = type
			this.box_id = id
			this.getDetails()
		}
	}
</script>
<style>
	page {
		background: #FFFFFF;
	}
</style>
<style lang="scss">
.shopList_head {
	height: 80rpx;
	color: #FA7E48;
	font-size: 28rpx;
	background: rgba(247, 157, 44, 0.1);
}
.shopList_ul {
	flex-wrap: wrap;
	overflow: hidden;
	padding: 30rpx 30rpx 120rpx 30rpx;
	.shopList_li {
		margin: 0 30rpx 30rpx 0;
		&:nth-child(3n) {
			margin-right: 0;
		}
		.shopList_li_name {
			width: 210rpx;
			height: 68rpx;
			display: -webkit-box;
			-webkit-box-orient: vertical;
			-webkit-line-clamp: 2;
			overflow: hidden;
			font-size: 26rpx;
		}
		.shopList_li_con {
			width: 210rpx;
			height: 204rpx;
			margin-bottom: 20rpx;
			flex-direction: column;
			position: relative;
			background: #F3F1F4;
			border-radius: 10rpx;
			.shopList_li_tag {
				top: 0;
				left: 0;
				color: #FFFFFF;
				font-size: 22rpx;
				width: 80rpx;
				height: 36rpx;
				z-index: 20;
				position: absolute;
				background:rgba(0, 0, 0, 0.5);
				border-radius: 10rpx 0rpx 20rpx 0rpx;
			}
			.orange {
				background: #FA7E48;
			}
			.red {
				background: #FF413F;
			}
			.shopList_li_img {
				width: 135rpx;
				height: 135rpx;
			}
			.shopList_li_price {
				display: flex;
				margin-top: 10rpx;
				align-items: flex-end;
				text {
					color: #CF271B;
					&:last-child {
						font-size: 28rpx;
					}
				}
			}
		}
	}
}
.shopList_footer {
	width: 100%;
	bottom: 0;
	left: 0;
	z-index: 20;
	padding-left: 60rpx;
	position: fixed;
	height: 120rpx;
	background: #FFFFFF;
	box-shadow: 0rpx 2rpx 5rpx 0rpx rgba(133, 159, 221, 0.6);
	.shopList_footer_li {
		flex: 1;
		display: flex;
		flex-direction: column;
		text {
			color: #FF413F;
			font-size: 30rpx;
			&:last-child {
				margin-top: 15rpx;
				font-size: 28rpx;
			}
		}
		&:nth-child(2) {
			text {
				color: #FA7E48;
			}
		}
		&:last-child {
			text {
				color: #999999;
			}
		}
	}
}
</style>
