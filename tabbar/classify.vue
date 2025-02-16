<template>
	<view class="classify">
		<view class="classify_search center">
			<view class="classify_search_ipt flexs" @click="goSearch">
				<image src="/static/image/home/sousuo@2x.png" mode=""></image>
				<input type="text" disabled placeholder="输入您要搜索的内容" placeholder-style="color:#808080" />
			</view>
		</view>
		<view class="classify_main flexs">
			<scroll-view scroll-y="true" class="classify_main_left scroll-view">
				<view class="classify_main_left_li center" :class="{active:index == i}" v-for="(item,index) in classList" :key="index" @click="changeIndex(index)">{{ item.name }}</view>
			</scroll-view>
			<scroll-view scroll-y="true" class="scroll-view classify_main_right">
				<view class="scroll-view_ul">
					<view class="classify_main_right_li flexs" v-for="(item,index) in boxList" :key="index" @click="goMessage(item)">
						<view class="classify_main_right_li_img">
							<image :src="item.image" mode="aspectFill"></image>
						</view>
						<view class="classify_main_right_li_main">
							<view class="classify_main_right_li_name">{{ item.box_name }}</view>
							<view class="classify_main_right_li_price">{{ item.coin_price }}金币</view>
						</view>
					</view>
				</view>
				
			</scroll-view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				i:0,//选择哪个分类
				page:1,//分页
				pages:null,//总页数
				boxList:[],//盒子列表
				classList:[],//分类列表
				category_id:'',//分类ID
			};
		},
		methods:{
			//去搜索
			goSearch () {
				uni.navigateTo({url:'/pages/index/search'})
			},
			//选择分类
			changeIndex (index) {
				this.i = index
				this.page = 1
				this.category_id = this.classList[index].category_id
				this.getBoxListByCategory()
			},
			//去详情
			goMessage (item) {
				uni.navigateTo({url:'/pages/index/details?id=' + item.box_id})
			},
			//获取一级分类
			getCategoryList () {
				this.$api.categoryList().then(res=>{
					if (res.code === 1) {
						this.classList = res.data
						this.category_id = res.data[this.i].category_id
						this.getBoxListByCategory()
					}
				})
			},
			//get
			getBoxListByCategory () {
				this.$api.boxListByCategory({page:this.page,pagesize:'10',category_id:this.category_id,msg:'数据加载中'}).then(res=>{
					if (res.code === 1) {
						this.boxList = this.page == 1 ? res.data.data : [...rhis.boxList,...res.data.data],
						this.pages = res.data.last_page
					}
				})
			}
		},
		onLoad() {
			this.getCategoryList()
		}
	}
</script>

<style lang="scss">
.classify_search {
	// position: fixed;
	width: 100%;
	height: 88rpx;
	// top: 0;
	// left: 0;
	padding: 0 30rpx;
	background: #FFFFFF;
	border-bottom: 2rpx solid #FAFAFA;
	.classify_search_ipt {
		width: 100%;
		padding: 0 20rpx;
		height: 58rpx;
		background: #F7F7F7;
		border-radius: 29rpx;
		image {
			width: 44rpx;
			height: 44rpx;
			margin-right: 15rpx;
		}
		input {
			flex: 1;
			font-size: 26rpx;
		}
	}
}
.scroll-view {
	height: calc(100vh - 88rpx - 50px);
	box-sizing: border-box;
}
.classify_main_left {
	width: 260rpx;
	background: #FFFFFF;
	.classify_main_left_li {
		height: 80rpx;
		font-size: 30rpx;
	}
	.active {
		color: #F6AF32;
		font-weight: bold;
		background: #F5F5F5;
	}
}
.classify_main_right {
	padding: 40rpx 0rpx 0rpx 30rpx;
	.scroll-view_ul {
	overflow: hidden;
	}
	.classify_main_right_li {
		padding-right: 30rpx;
		margin-bottom: 30rpx;
		&:last-child {
			// margin-bottom: 0;
		}
		.classify_main_right_li_img {
			image {
				width: 120rpx;
				height: 120rpx;
				background: #ECEEFB;
				border-radius: 10rpx;
			}
			margin-right: 20rpx;
		}
		.classify_main_right_li_main {
			
			.classify_main_right_li_name {
				// width: 284rpx;
				display: -webkit-box;
				-webkit-box-orient: vertical;
				-webkit-line-clamp: 2;
				overflow: hidden;
				margin-bottom: 20rpx;
			}
			.classify_main_right_li_price {
				color: #CF271B;
				font-size: 26rpx;
			}
		}
	}
}
</style>
