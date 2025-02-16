<template>
	<view class="set">
		<view class="set_ul">
			<view class="set_li flex">
				<view class="set_li_name">头像</view>
				<view class="set_li_img flexs" @click="uploadHead">
					<image :src="info.avatar" mode="aspectFill" class="head"></image>
					<image src="/static/image/publice/jinruer@2x.png" mode=""></image>
				</view>
			</view>
			<view class="set_li flex">
				<view class="set_li_name">昵称</view>
				<view class="set_li_ipt">
					<input type="text" @input="changeName" :value="info.nickname" />
				</view>
			</view>
		</view>
		<button class="set_btn" hover-class="hover-view" @click="quitLogin">退出登录</button>
	</view>
</template>

<script>
	import $DB from '../../http/debounce.js'
	export default {
		data() {
			return {
				info:{},//
			};
		},
		methods:{
			//退出登录
			quitLogin () {
				uni.showModal({
					content: '是否退出登录？',
					success:  (res) => {
						if (res.confirm) {
							uni.removeStorageSync('token')
							uni.redirectTo({url:'/pages/login/login'})
						}
					}
				});
				
			},
			//修改名字
			changeName:$DB(function(e){
				if (!e.detail.value) return
				this.$api.changeInfo({nickname:e.detail.value}).then(res=>{
					if (res.code === 1) {
						
					}
				})
			}),
			//修改头像
			uploadHead () {
				uni.chooseImage({
					count: 1, //默认9
					sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
					sourceType: ['album','camera'], //从相册选择
					success:  (res)=> {
						this.$api.upload_image({path:res.tempFilePaths[0]}).then(res=>{
							this.$api.changeInfo({avatar_url:res.url}).then(res=>{
								if (res.code === 1) {
									uni.showToast({title:res.msg})
									this.getSet()
								}
							})
						})
					}
				});
			},
			//获取设置信息
			getSet () {
				this.$api.getSettingInfo().then(res=>{
					if (res.code === 1) {
						this.info = res.data
					}
				})
			}
		},
		onShow() {
			this.getSet()
		}
	}
</script>

<style lang="scss">
	.set {
		min-height: calc(100vh - 44px);
		position: relative;
	}
.set_li {
	padding: 30rpx;
	background: #FFFFFF;
	margin-top: 2rpx;
	&:first-child {
		padding: 15rpx 30rpx;
	}
	.set_li_name {
		font-size: 28rpx;
	}
	.set_li_img {
		image {
			width: 22rpx;
			height: 22rpx;
		}
		.head {
			width: 70rpx;
			height: 70rpx;
			flex-shrink: 0;
			margin-right: 20rpx;
			border-radius: 50%;
		}
	}
	.set_li_ipt {
		color: #999999;
		font-size: 26rpx;
		input {
			font-size: 26rpx;
			text-align: right;
		}
	}
}
.set_btn {
	color: #333333;
	height: 98rpx;
	bottom: 42rpx;
	left: 50%;
	width: 690rpx;
	position: absolute;
	transform: translateX(-50%);
	font-size: 30rpx;
	background: #FFFFFF;
	box-shadow: 0rpx 0rpx 16rpx 0rpx rgba(102, 102, 102, 0.2);
	border-radius: 10rpx;
}
</style>
