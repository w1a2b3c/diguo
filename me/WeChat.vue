<template>
	<view class="alipay">
		<view class="alipay_ul">
			<view class="alipay_li flex">
				<text>真实姓名</text>
				<input type="text" placeholder="请填写真实姓名" v-model="user.username" placeholder-style="color:#999999" />
			</view>
			<view class="alipay_li flex">
				<text>{{ user.type == 'wechat' ? '微信账号' : '支付宝账号' }}</text>
				<input type="text" v-model="user.account" :placeholder="user.type == 'wechat' ? '请填写微信账号' : '请填写支付宝账号' " placeholder-style="color:#999999" />
			</view>
		</view>
		<button hover-class="hover-view" class="alipay_btn" @click="submit">提交</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				user:{
					type:'wechat',
					username:'',
					account:''
				}
			};
		},
		methods:{
			submit () {
				if (!this.user.username) return uni.showToast({title:'请填写真实姓名',icon:'none'})
				if (!this.user.account) return uni.showToast({title:this.user.type == 'wechat' ? '请填写微信账号' : '请填写支付宝账号',icon:'none'})
				this.$api.bindWithdrawalAccount(this.user).then(res=>{
					if (res.code === 1) {
						uni.showToast({title:res.msg})
						setTimeout(()=>{
							uni.navigateBack()
						},800)
					}
				})
			},
			getAccount () {
				this.$api.getWithdrawalSetting().then(res=>{
					if (res.code === 1) {
						this.user.username = this.user.type == 'wechat' ? res.data.wechat.username : res.data.alipay.username
						this.user.account = this.user.type == 'wechat' ? res.data.wechat.account : res.data.alipay.account
					}
				})
			}
		},
		onLoad ({type}) {
			this.user.type = type == 0 ? 'wechat' : 'alipay'
			uni.setNavigationBarTitle({title:type == 0 ? '微信绑定' : '支付宝绑定'});
			this.getAccount()
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
