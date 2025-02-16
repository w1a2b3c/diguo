<template>
	<view class="box">
		<view class="box_head flexs">
			<view class="box_head_li center" @click="changeIndex(index)" v-for="(item,index) in 2" :class="{active:index == i}" :key="index">{{ index == 0 ? '待提货' : '已回收' }}</view>
		</view>
		<view class="box_ul">
			<view class="box_ul_li flexs" v-for="(item,index) in boxList" :key="index"  @click="item.flag = !item.flag">
				<view class="box_ul_li_img">
					<image :src="item.goods_image" mode="aspectFill"></image>
				</view>
				<view class="box_ul_li_main">
					<view class="box_ul_li_name">{{ item.goods_name }}</view>
					<view class="box_ul_li_price flex">
						<text>{{ item.box_coin_price }}金币</text>
						<image v-if="i == 0" :src="item.flag ? '/static/image/publice/xuanzhong1@2x.png' : '/static/image/me/weixuanzhong@2x.png'" mode=""></image>
					</view>
					<view class="box_ul_li_time">时间：{{ item.time }}</view>
				</view>
			</view>
		</view>
		<view class="box_footer flex" v-if="i == 0">
			<view class="box_footer_all flexs">
				<image @click="changeAll" :src="isAll ? '/static/image/publice/xuanzhong1@2x.png' : '/static/image/me/weixuanzhong@2x.png'" mode=""></image>
				<text>全选({{ totalNumber }})</text>
			</view>
			<view class="box_footer_ul flexs">
				<view class="box_footer_li center" v-for="(items,index) in 2" :key="index" @click="apply(index)"> {{ index == 0 ? '一键回收' : '申请发货' }} </view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				i:0,//
				page:1,
				isAll:false,//
				pages:null,//
				totalNumber:0,//总数量
				boxList:[],//盒子列表
			};
		},
		watch:{
			boxList:{
				handler(val){
					let totalNumber = 0
					
					this.isAll = val.every(item=>item.flag)
					if (val.length == 0) this.isAll = false
					val.forEach(item=>{
						if (item.flag) {
							totalNumber++
						}
					})
					this.totalNumber = totalNumber
				},
				deep:true //true 深度监听
			}
		},
		methods:{
			//申请发货
			apply (index) {
				if (!this.boxList.some(item=>item.flag)) return uni.showToast({title:'请勾选要' +( index == 0 ? '回收' : '发货') +  '的商品',icon:'none'})
				let ids = []
				this.boxList.forEach(item=>{
					if (item.flag) {
						ids.push(item.record_id)
					}
				})
				if (index == 0) {
					this.$api.exchange({record_ids:ids.join(',')}).then(res=>{
						if (res.code === 1) {
							uni.showToast({title:res.msg})
							this.page = 1
							this.getMyBox()
						}
					})
				} else {
					uni.navigateTo({url:'/pages/me/applyFaHuo?id=' + ids.join(',')})
				}
			},
			//全选
			changeAll () {
				this.isAll = !this.isAll
				this.boxList.forEach(item=>{
					item.flag = this.isAll
				})
			},
			changeIndex (index) {
				this.i = index
				this.page = 1
				this.boxList = []
				this.getMyBox()
			},
			//获取我的盒柜
			getMyBox () {
				this.$api.myBox({status:this.i + 1,page:this.page,msg:'数据加载中'}).then(res=>{
					if (res.code === 1) {
						res.data.data.forEach(item=>{
							item.flag = false
						})
						this.pages = res.data.last_page
						this.boxList = this.page == 1 ? res.data.data : [...this.boxList,...res.data.data]
					}
				})
			}
		},
		onShow () {
			this.getMyBox()
		},
		onReachBottom() {
			if (this.page < this.pages) {
				this.page++
				this.getMyBox()
			}
		}
	}
</script>

<style lang="scss">
.box_head {
	position: sticky;
	top: 88rpx;
	left: 0;
	background: #FAFAFA;
	z-index: 2021;
	width: 100%;
	.box_head_li {
		flex:1 ;
		font-size: 30rpx;
		height: 90rpx;
		font-weight: bold;
	}
	.active {
		color: #F6AF32;
	}
}
.box_ul {
	padding: 0 30rpx 98rpx 30rpx;
	.box_ul_li {
		padding: 30rpx;
		margin-bottom: 20rpx;
		background: #FFFFFF;
		border-radius: 20rpx;
		.box_ul_li_img {
			image {
				width: 168rpx;
				height: 168rpx;
				border-radius: 10rpx;
			}
			margin-right: 20rpx;
		}
		.box_ul_li_main {
			flex: 1;
		}
		.box_ul_li_name {
			font-size: 28rpx;
			display: -webkit-box;
			-webkit-box-orient: vertical;
			-webkit-line-clamp: 2;
			overflow: hidden;
		}
		.box_ul_li_price {
			text {
				color: #CF271B;
				font-size: 28rpx;
			}
			image {
				width: 44rpx;
				height: 44rpx;
			}
			margin: 15rpx 0;
		}
		.box_ul_li_time {
			color: #999999;
			font-size: 22rpx;
		}
	}
}
.box_footer {
	height: 98rpx;
	z-index: 20;
	position: fixed;
	bottom: 50px;
	width: 100%;
	padding: 0 30rpx;
	background: #FFFFFF;
	.box_footer_all {
		image {
			width: 44rpx;
			height: 44rpx;
			margin-right: 10rpx;
		}
		text {
			color: #000000;
			font-size: 28rpx;
		}
	}
	.box_footer_li {
		color: #FFFFFF;
		width: 220rpx;
		height: 78rpx;
		border-radius: 39rpx;
		font-size: 30rpx;
		margin-left: 30rpx;
		&:first-child {
			background: #FA7E48;
		}
		&:last-child {
			background: #F6AF32;
		}
	}
}
</style>
