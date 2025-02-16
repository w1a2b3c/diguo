<template>
	<view class="service">
		<view class="service_ul">
			<scroll-view scroll-y="true" refresher-default-style="none" @scrolltoupper="scrolltoupper" refresher-background="rgba(255,255,255,0)" id="scrollview" scroll-with-animation class="scroll-view" :scroll-top="scrollTop" :scroll-into-view="indexId">
				<view id="msglistview">
					<view class="service_ul_li" v-for="(item,index) in list" :key="index" >
						<view class="service_ul_left flexs" v-if="index % 2 == 0" :id="'index' + index">
							<view class="service_ul_left_ttx">这次工作很重要</view>
							<view class="service_ul_li_img">
								<image src="/static/logo.png" mode="aspectFill"></image>
							</view>
						</view>
						<view class="service_ul_right flexs" v-else>
							<view class="service_ul_li_img">
								<image src="/static/logo.png" mode="aspectFill"></image>
							</view>
							<view class="service_ul_right_txt">您还有其他交代吗您还有其他交代吗您还有其他交代吗？</view>
						</view>
					</view>
				</view>
				
			</scroll-view>
		</view>
		<view class="service_footer flex">
			<view class="service_footer_ipt flexs">
				<textarea auto-height maxlength="-1" v-model="keyword" @focus="changefocus" :focus="focus" hold-keyboard @confirm="submitSend" fixed confirm-type="send" />
			</view>
			<!-- <view class="service_footer_send center" @click="submitSend">发送</view> -->
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				keyword:'',
				focus:true,
				indexId:'index0',
				scrollTop:40,
				list:[0,1,2,3,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],//
			};
		},
		methods:{
			scrolltoupper (e) {
				console.log(e);
				this.list = [0,...this.list]
			},
			changefocus () {
				 let u = navigator.userAgent, app = navigator.appVersion;
				   let isAndroid = u.indexOf('Android') > -1 || u.indexOf('Linux') > -1;
				   if(isAndroid){
				      setTimeout(function() {
				      document.activeElement.scrollIntoViewIfNeeded();
				      document.activeElement.scrollIntoView();
				      }, 500);
				   }
			},
			//获取页面高度
			getHeight () {
				setTimeout(()=>{
					let that = this
					let query = uni.createSelectorQuery()
					query.select('#scrollview').boundingClientRect()
					query.select('#msglistview').boundingClientRect()
					query.exec((res) => {
							if(res[1].height > res[0].height){
									that.scrollTop = res[1].height
							}
					})
				},100)
			},
			submitSend () {
				this.list.push(0)
				this.indexId = 'index' + (this.list.length - 1)
				this.focus = true
				this.keyword = ''
				this.getHeight()
			}
		},
		onLoad() {
			this.getHeight()
			setInterval(()=>{
				this.list.push(0)
				this.getHeight()
			},3000)
		}
	}
</script>

<style lang="scss">
	#msglistview {
		overflow: hidden;
	}
	.service_ul {
		
		// padding-right: 30rpx;
	}
.scroll-view {
	width: 770rpx;
	overflow: hidden;
	padding: 0 0 0 30rpx;
	
	background: #FAFAFA;
	height: calc(100vh - 98rpx - 44px);
	.service_ul_left {
		display: flex;
		justify-content: flex-end;
	}
	.service_ul_left_ttx {
		padding: 20rpx;
		font-size: 32rpx;
		position: relative;
		background: #95EC69;
		border-radius: 10rpx;
		margin-right: 20rpx;
		&::after {
			content: '';
			top: 50%;
			right: -17rpx;
			transform: translateY(-50%);
			position: absolute;
			border-width: 9rpx;
			border-color: transparent transparent transparent #95EC69;
			border-style: dotted dotted dotted solid;
		}
	}
	.service_ul_right_txt {
		padding: 20rpx;
		font-size: 32rpx;
		position: relative;
		background: #FFFFFF;
		border-radius: 10rpx;
		margin-left: 20rpx;
		&::after {
			content: '';
			top: 50%;
			left: -17rpx;
			transform: translateY(-50%);
			position: absolute;
			border-width: 9rpx;
			border-color: transparent  #FFFFFF transparent transparent ;
			border-style: dotted dotted dotted solid;
		}
	}
	.service_ul_li {
		padding-right: 50rpx;
		margin-bottom: 40rpx;
		&:first-child {
			margin-top: 40rpx;
		}
	}
	.service_ul_li_img {
		width: 80rpx;
		height: 80rpx;
		flex-shrink: 0;
		image {
			border-radius: 4rpx;
		}
		
	}
}
.service_footer {
	width: 100%;
	position: fixed;
	bottom: 0;
	background: #FFFFFF;
	padding: 20rpx 0 20rpx 30rpx;
	.service_footer_ipt {
		width: 605rpx;
		min-height: 56rpx;
		padding: 10rpx 20rpx;
		background: #FAFAFA;
		border-radius: 28rpx;
	}
	.service_footer_send {
		font-size: 28rpx;
		flex: 1;
		height: 56rpx;
	}
}
</style>
