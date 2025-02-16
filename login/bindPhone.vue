<template>
	<view class="register">
		<view class="register_head">
			<text>绑定手机号</text>
			<text>更好为您服务，保障信息安全，请绑定手机号。</text>
		</view>
		<view class="register_ul">
			<view class="register_li flex">
				<view class="register_ipt">
					<input type="number" maxlength="11" v-model="from.mobile" placeholder="请输入手机号" placeholder-style="color:#999999" />
				</view>
			</view>
			<view class="register_li flex">
				<view class="register_ipt">
					<input type="password" maxlength="12" placeholder="请输入密码" v-if="isPassword" placeholder-style="color:#999999" />
					<input type="number" maxlength="6" v-model="from.captcha" placeholder="请输入验证码" v-else placeholder-style="color:#999999" />
				</view>
				<view class="code center" v-if="!isPassword" @click="getCode">{{ codeTxt }}</view>
			</view>
		</view>
		<view class="register_code" @click="isPassword = !isPassword" v-if="isPassword">验证码登录</view>
		<button class="register_btn" hover-class="hover-view" @click="submit">确定</button>
	</view>
</template>

<script>
	import $DB from '../../http/debounce.js'
	export default {
		data() {
			return {
				isPassword:false,//是否密码登录
				isConsent:false,//是否同意
				flag:true,
				codeTxt:'获取验证码',
				sending:true,
				second:60,
				disabled:false,
				from:{
					mobile:'',//
					captcha:'',//
				}
			};
		},
		methods:{
			//去隐私协议
			goPrivacy () {
				uni.navigateTo({url:'/pagesA/pages/privacy'})
			},
			//去用户协议
			goUser () {
				uni.navigateTo({url:'/pagesA/pages/user'})
			},
			//登录
			submit:$DB(function(){
				if (!this.from.mobile.match(/^(0|86|17951)?1[3456789]\d{9}$/)) {
					uni.showToast({
						title: '请输入正确的手机号',
						icon:'none'
					});
					return
				}
				if (!this.from.captcha) return uni.showToast({title:'请输入验证码',icon:'none'})
				if (!this.flag) return
				this.flag = false
				this.$api.bindMobile({...this.from,msg:'绑定中'}).then(res=>{
					if (res.code === 1) {
						uni.switchTab({url:'/pages/tabbar/index'})
					} else {
						this.flag = true
					}
				}).then(res=>{
					console.log(res);
				})
			}),
			//验证码登录
			codeLogin () {
				uni.navigateTo({url:'/pages/login/phoneLogin'})
			},
			//注册账号
			goRegister () {
				uni.navigateTo({url:'/pages/login/register'})
			},
			//忘记密码
			goPassword () {
				if (this.isPassword) {
					uni.navigateTo({url:'/pages/login/forgetPassword'})
				} else {
					this.isPassword = true
				}
				
			},
			//获取验证码
			getCode:$DB(function(){
				if (!this.sending) return
				if (!this.from.mobile.match(/^(0|86|17951)?1[3456789]\d{9}$/)) {
					uni.showToast({
						title: '请输入正确的手机号',
						icon:'none'
					});
					return
				}
				this.$api.send({mobile:this.from.mobile,msg:'发送中',event:'bindmobile'}).then( res => {
					this.sending = false;
					this.disabled = true;
					if (res.code === 1) {
						this.timeDown();
					}
					uni.showToast({
						title: res.msg,
						icon:'none'
					});
				});
			}),
			timeDown(){
				let result = setInterval( ()=>{
					--this.second;
					this.codeTxt = this.second + 'S'
					if(this.second < 0) {
						clearInterval(result);
						this.sending = true;
						this.disabled = false;
						this.second = 60;
						this.codeTxt = '获取验证码'
					} 
				}, 1000);
			},
		},
		onLoad ({token,is_channel, is_notice}) {
			if (token) {
				uni.setStorageSync('token',token)
			}
      if (is_channel) {
      	uni.setStorageSync('is_channel',is_channel)
      }
      if (is_notice) {
      	uni.setStorageSync('is_notice', is_notice)
      }
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
	margin-top: 125rpx;
	background: #F6AF32;
	border-radius: 40rpx;
}
.register_code {
	color: #4EABFC;
	font-size: 28rpx;
	text-align: right;
	margin: 30rpx 0 0rpx 0;
}
</style>
