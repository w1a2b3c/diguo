<!--  -->
<template>
	<view class="details">
	<audio id="audio" src="/h5/pagesA/static/bj1.mp3" loop></audio>
    <!-- nav start -->
    <view class="details_title">
    	<view class="details_nav"></view>
    	<view class="details-title_head flex">
    		<view class="details_title_return" @click="returnTop">
    			<image src="/static/image/publice/fanhui@2x.png" mode=""></image>
    		</view>
    		<view class="details_title_name">{{ boxDetail.box_name }}</view>
    		<view class="details_title_mp3" @click="changePlay">
    			<image :src="isPlay ? '/static/image/home/bofang@2x.png' : '/static/image/home/guanbiyinyue@2x.png' " mode=""></image>
    		</view>
    	</view>
    </view>
    <!-- nav end -->
    <!-- 飘屏start -->
    <view class="pp">
      <view class="btn-view flex">
        <view class="btn" v-for="item in 2" :key="item" @tap="changeDraw(item-1)">
          <image :src="`../../static/image/box/btn${item}@2x.png`" mode="aspectFit"></image>
          <text class="price">{{item==1?boxDetail.coin_price : (item==2?boxDetail.coin_price*5:boxDetail.coin_price*10)}}元</text>
        </view>
      </view>
      <!-- 弹幕start -->
      <!-- <lff-barrage ref="lffBarrage" :minTop="0" :maxTop="106"></lff-barrage> -->
      <!-- 弹幕end -->
      
      <!-- 飘屏项start -->
      <!-- <view class="pp-item">
        <view class="avatar">
          <image src="../../static/image/me/weixin@2x.png" mode=""></image>
        </view>
        <view class="text-view">
          <text class="username">公主**龙</text>
          <text class="goods-name">抽中了 大油田*1</text>
        </view>
      </view> -->
      <!-- 飘屏项end -->
    </view>
    <!-- 飘屏end -->
    <!-- 商品预览start -->
    <view class="goods-view">
      <view class="title flex">
        <text class="title-left">商品预览</text>
		<view class="details_footer_coll" @click="changeStar" v-if="type == 0">
			<image :src=" boxDetail.is_star == 1 ? '/static/image/home/shoucangyixuanzhong@2x.png' : '/static/image/home/shoucangweixuanzhong@2x.png' " mode=""></image>
		</view>
        <view class="title-right flex" @tap="$refs.regulation.open()">
          <text>规则玩法</text>
          <view class="rule-box">
            <image src="../../static/image/box/rule@2x.png" mode="aspectFit"></image>
          </view>
        </view>
      </view>
      <view class="rate-view flex">
        <text class="rate-txt">获得概率</text>
        <view class="rate-list flex">
          <view class="rate-item" v-for="(item, index) in rates" :key="item.value">
            <text class="rate-value">{{item.value}}</text>
            <text :class="['rate-name', `rate-name-${index}`]">{{item.name}}</text>
          </view>
        </view>
      </view>
      <view class="goods-list-view flex">
        <view class="goods-item" v-for="(item, index) in boxDetail.goodslist" :key="index">
          <text class="goods-tag" :class="`goods-tag-${item.tag=='稀有'?1:(item.tag=='珍贵'?2:3)}`">{{item.tag}}</text>
          <view class="goods-img-view">
            <image :src="item.image" mode="aspectFit"></image>
          </view>
          <view class="goods-price-view">
            参考价 ￥{{item.price}}
          </view>
          <view class="goods-name">{{item.goods_name}}</view>
        </view>
      </view>
    </view>
    <!-- 商品预览end -->
    <!-- 开盒记录start -->
    <view class="record-view">
      <view class="title flex">
        <text class="title-left">开盒记录</text>
        <view class="title-right flex" @tap="$refs.morerecord.open()">
          <text>更多记录</text>
          <view class="rule-box">
            <image src="../../static/image/publice/jinruer@2x.png" mode="aspectFit"></image>
          </view>
        </view>
      </view>
      <view class="record-list-view">
        <view class="record-item flex" v-for="(item, index) in record3" :key="index">
          <view class="goods-img-view">
            <image :src="item.goods_image" mode="aspectFit"></image>
          </view>
          <view class="record-item-right">
            <view class="goods-name-view flex">
              <text class="username">{{item.nickname}}</text>
              <text class="txt">开出了</text>
              <text class="goods-name">{{item.goods_name}}</text>
            </view>
            <view class="goods-price-view">
              参考价 ￥{{item.goods_rmb_price}}
            </view>
            <view class="goods-time-view">
              开盒时间：{{item.create_time}}
            </view>
          </view>
        </view>
      </view>
    </view>
    <!-- 开盒记录end -->
    <!-- 购买须知start -->
    <view class="buy-rule-view">
      <view class="title flex">
        <text class="title-left">购买须知</text>
      </view>
      <view class="box-view" v-html="notice_to_buyers">
        <!-- <view class="rule-item">
          <view class="rule-title">
            【盲盒规则】
          </view>
          <view class="rule-html">
            这是一段规则这是一段规则这是一段规则这是一段规则这是一段规则这是一段规则这是一段规则这是一段规则这是一段规则这是一段规则
          </view>
        </view>
        <view class="rule-item">
          <view class="rule-title">
            【关于配送】
          </view>
          <view class="rule-html">
            这是一段规则这是一段规则这是一段规则这是一段规则这是一段规则这是一段规则这是一段规则这是一段规则这是一段规则这是一段规则
          </view>
        </view>
        <view class="rule-item">
          <view class="rule-title">
            【售后问题】
          </view>
          <view class="rule-html">
            这是一段规则这是一段规则这是一段规则这是一段规则这是一段规则这是一段规则这是一段规则这是一段规则这是一段规则这是一段规则
          </view>
        </view> -->
      </view>
    </view>
    <!-- 购买须知end -->
    
		<!--S  玩法规则 -->
		<uni-popup ref="regulation" :mask-click="false">
		  <view class="regulation">
				<view class="regulation_close" @click="$refs.regulation.close()">
					<image src="/static/image/home/guanbi@2x.png" mode=""></image>
				</view>
		  	<view class="regulation_name">玩法规则</view>
				<view class="regulation_main">
					<scroll-view scroll-y="true" class="scroll-view">
						<u-parse :content="message" ></u-parse>
					</scroll-view>
				</view>
		  </view>
		</uni-popup>
		<!--E  玩法规则 -->
    
    <!--S  更多记录 -->
    <uni-popup ref="morerecord" :mask-click="true">
      <view class="regulation">
    		<view class="regulation_close" @click="$refs.morerecord.close()">
    			<image src="/static/image/home/guanbi@2x.png" mode=""></image>
    		</view>
      	<view class="regulation_name">开盒记录</view>
    		<view class="regulation_main">
    			<scroll-view scroll-y="true" class="scroll-view">
    				<view class="record-item flex" v-for="(item, index) in boxDetail.record" :key="index">
              <view class="goods-img-view">
                <image :src="item.goods_image" mode="aspectFit"></image>
              </view>
              <view class="record-item-right">
                <view class="goods-name-view flex">
                  <text class="username">{{item.nickname}}</text>
                  <text class="txt">开出了</text>
                  <text class="goods-name">{{item.goods_name}}</text>
                </view>
                <view class="goods-price-view">
                  参考价 ￥{{item.goods_rmb_price}}
                </view>
                <view class="goods-time-view">
                  开盒时间：{{item.create_time}}
                </view>
              </view>
            </view>
    			</scroll-view>
    		</view>
      </view>
    </uni-popup>
    <!--E  更多记录 -->
    
	</view>
</template>

<script>
	const innerAudioContext = uni.createInnerAudioContext();
	innerAudioContext.autoplay = true;
	innerAudioContext.loop = true;
	innerAudioContext.src = '/static/image/home/bj.mp3';
	
	export default {
		data() {
			return {
        timer: null,
				type:null,//1试玩0详情
				box_id:'',//盲盒ID
				message:'',//玩法规则
				boxDetail:{},//盲盒详情
        record3: [],
				isPlay:true,
        notice_to_buyers:"",
        rates: [
          {
            value: '0.2%',
            name: '稀有款'
          },
          {
            value: '10.8%',
            name: '珍贵款'
          },
          {
            value: '89.0%',
            name: '普通款'
          }
        ]
			};
		},
		methods:{
			lookImg (url) {
				uni.previewImage({
						urls: [url],
						longPressActions: {
								itemList: ['发送给朋友', '保存图片', '收藏'],
								success: function(data) {
										console.log('选中了第' + (data.tapIndex + 1) + '个按钮,第' + (data.index + 1) + '张图片');
								},
								fail: function(err) {
										console.log(err.errMsg);
								}
						}
				});
			},
			changePlay () {
				this.isPlay = !this.isPlay
				uni.setStorageSync('isPlay',this.isPlay)
				this.isPlay ? innerAudioContext.play() : innerAudioContext.pause()
			},
			//去充值
			recharge () {
				uni.navigateTo({url:'/pages/me/recharge'})
			},
			//是否点赞
			changeStar () {
				if (this.type == 1) return uni.showToast({title:'试玩商品不可操作',icon:'none'})
				this.$api.star({box_id:this.box_id}).then(res=>{
					if (res.code === 1) {
						res.data.is_star == 1 ? this.boxDetail.is_star = 1 : this.boxDetail.is_star = 0
						uni.showToast({title:res.msg})
					}
				})
			},
			//去抽奖
			changeDraw (index) {
				innerAudioContext.pause()
        // if (index == 0) {
        //   index = 1
        // } else if (index == 1) {
        //   index = 0
        // }
				uni.navigateTo({url:'/pagesA/pages/camera?type=' + this.type + '&index=' + index + '&id=' + (this.type== 1 ? this.boxDetail.box_id : this.box_id) + '&name=' + this.boxDetail.box_name})
			},
			//是否点赞
			changeStar () {
				if (this.type == 1) return uni.showToast({title:'试玩商品不可操作',icon:'none'})
				this.$api.star({box_id:this.box_id}).then(res=>{
					if (res.code === 1) {
						res.data.is_star == 1 ? this.boxDetail.is_star = 1 : this.boxDetail.is_star = 0
						uni.showToast({title:res.msg})
					}
				})
			},
			//去更多商品
			goMore () {
				uni.navigateTo({url:'/pages/index/shopList?id=' + this.box_id + '&type=' + this.type})
			},
			returnTop () {
				uni.navigateBack()
			},
			//获取详情
			getDetails () {
				this.$api[this.type == 1 ? 'tryBoxDetail' : 'boxDetail' ]({box_id:this.box_id}).then(res=>{
					if (res.code === 1) {
						this.boxDetail = res.data
            this.record3 = res.data.record.slice(0, 3)
					}
				})
			},
			//玩法规则
			getMessage () {
				this.$api.agreement({name:'play_rule'}).then(res=>{
					if (res.code === 1) {
						this.message = res.data.content
					}
				})
        this.$api.agreement({name:'notice_to_buyers'}).then(res=>{
        	if (res.code === 1) {
        		this.notice_to_buyers = res.data.content
        	}
        })
			}
		},
		onUnload() {
			innerAudioContext.pause()
      console.log('onUnload')
      // clearInterval(this.timer)
		},
		onLoad({id,type}) {
			this.type = type
			this.box_id = id
			this.getDetails()
			this.getMessage()
		},
		onShow() {
			this.isPlay = uni.getStorageSync('isPlay')
			this.isPlay ? innerAudioContext.play() : innerAudioContext.pause()
    },
	mounted(){
		let audio = document.getElementById("audio").querySelector("audio")
		audio.play()
	},
    onReady () {
      // this.timer = setInterval(() => {
      //   this.$refs.lffBarrage.add({
      //     goods_name: '大游艇*1',
      //     username: '公主**龙',
      //     avatar: 'https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=317894666,3379114684&fm=26&gp=0.jpg'
      //   })
      // }, 1500)
    }
	}
</script>

<style lang="scss" scoped>
.details_footer_coll {
	width: 54rpx;
	height: 54rpx;
	}
.details_title {
	width: 100%;
	position: sticky;
	top: 0;
	z-index: 2021;
	.details_nav {width: 100%;}
	.details-title_head {
		width: 100%;
		height: 88rpx;
		padding: 0 30rpx;
		background: #FFFFFF;
		.details_title_return {
			width: 44rpx;
			height: 44rpx;
		}
		.details_title_name {
			width: 466rpx;
			text-align: center;
			overflow: hidden;
			text-overflow:ellipsis;
			white-space: nowrap;
			font-size: 36rpx;
			font-weight: bold;
		}
		.details_title_mp3 {
			width: 44rpx;
			height: 44rpx;
		}
	}
}
.regulation {
	width: 650rpx;
	height: 900rpx;
	padding: 0 0 0 30rpx;
	background: #FFFFFF;
	border-radius: 20rpx;
	position: relative;
	.regulation_close {
		position: absolute;
		width: 44rpx;
		height: 44rpx;
		top: 30rpx;
		right: 30rpx;
	}
	.regulation_name {
		padding: 40rpx 0;
		text-align: center;
		font-size: 30rpx;
		font-weight: bold;
	}
  .scroll-view {
  	height: 735rpx;
  }
}

.pp {
  background-image: url(../../static/image/box/bg@2x.png);
  width: 100%;
  height: 956rpx;
  background-repeat: no-repeat;
  background-position: center;
  background-size: 100% 100%;
  position: relative;
  .btn-view {
    position: absolute;
    width: 100%;
    bottom: 56rpx;
    padding: 0 48rpx;
    justify-content: space-around;
    .btn {
      width: 198rpx;
      height: 102rpx;
      position: relative;
      .price{
        position: absolute;
        width: 100%;
        text-align: center;
        font-size: 24rpx;
        font-family: PingFangSC-Regular, PingFang SC;
        font-weight: 400;
        color: #000000;
        line-height: 34rpx;
        bottom: 22rpx;
      }
    }
  }
  .pp-item {
    top: 40rpx;
    left: 40rpx;
    background-image: url(../../static/image/box/pp@2x.png);
    background-position: center;
    background-repeat: no-repeat;
    background-size: 100% 100%;
    width: 434rpx;
    height: 40rpx;
    line-height: 40rpx;
    position: absolute;
    padding-left: 52rpx;
    .avatar {
      position: absolute;
      z-index: 2;
      left: -10rpx;
      top: 50%;
      transform: translateY(-50%);
      width: 44rpx;
      height: 44rpx;
      border-radius: 50%;
      border: 2rpx solid #FFF;
      background: #D8D8D8;
    }
    .text-view {
      .username {
        font-size: 24rpx;
        font-family: PingFangSC-Regular, PingFang SC;
        font-weight: 400;
        color: #FFC50F;
        line-height: 34rpx;
        margin-right: 22rpx;
      }
      .goods-name {
        font-size: 24rpx;
        font-family: PingFangSC-Regular, PingFang SC;
        font-weight: 400;
        color: #FFFFFF;
        line-height: 34rpx;
      }
    }
  }
}

.goods-view {
  .title {
    padding: 0 30rpx;
    margin: 30rpx 0 22rpx;
    .title-left {
      font-size: 34rpx;
      font-family: PingFangSC-Medium, PingFang SC;
      font-weight: 500;
      color: #171A20;
      line-height: 48rpx;
    }
    .title-right {
      font-size: 24rpx;
      font-family: PingFangSC-Regular, PingFang SC;
      font-weight: 400;
      color: #7A808D;
      line-height: 34rpx;
      .rule-box {
        width: 28rpx;
        height: 28rpx;
        margin-left: 6rpx;
      }
    }
  }
  .rate-view {
    height: 80rpx;
    background: #FFF3CC;
    .rate-txt {
      display: inline-block;
      width: 172rpx;
      height: 80rpx;
      line-height: 80rpx;
      text-align: center;
      background: #FFCB0F;
      font-size: 24rpx;
      font-family: PingFangSC-Regular, PingFang SC;
      font-weight: 400;
      color: #0C0D10;
    }
    .rate-list {
      padding: 0 20rpx;
      justify-content: space-around;
      flex: 1;
      .rate-item {
        display: flex;
        flex-direction: column;
        align-items: center;
        .rate-value {
          font-size: 24rpx;
          font-family: PingFangSC-Regular, PingFang SC;
          font-weight: 400;
          color: #0C0D10;
          line-height: 34rpx;
        }
        .rate-name {
          font-size: 24rpx;
          font-family: PingFangSC-Medium, PingFang SC;
          font-weight: 500;
          line-height: 34rpx;
        }
      }
      .rate-name-0 {
        color: #f60000;
      }
      .rate-name-1 {
        color: #FF8C00;
      }
      .rate-name-2 {
        color: #5E94FF;
      }
    }
  }
  .goods-list-view {
    flex-wrap: wrap;
    padding: 0 30rpx;
    margin-top: 14rpx;
    &::after {
      content: '';
      width: 220rpx;
    }
    .goods-item {
      width: 220rpx;
      height: 256rpx;
      background: #FFFFFF;
      border-radius: 16rpx;
      position: relative;
      margin-top: 16rpx;
      .goods-tag {
        position: absolute;
        z-index: 2;
        right: 0;
        top: 0;
        width: 76rpx;
        text-align: center;
        height: 38rpx;
        line-height: 38rpx;
        color: #FFF;
        font-size: 24rpx;
        border-radius: 0 16rpx 0 16rpx;
        &-1 {
          background-color: #f60000;
        }
        &-2 {
          background-color: #FF8C00;
        }
        &-3 {
          background-color: #5E94FF;
        }
      }
      .goods-img-view {
        width: 158rpx;
        height: 158rpx;
        margin: 22rpx auto 34rpx;
      }
      .goods-price-view {
        position: absolute;
        top: 154rpx;
        left: 0;
        width: 190rpx;
        height: 40rpx;
        border-radius: 0px 24rpx 0px 0px;
        background: #FF495D;
        font-size: 20rpx;
        font-family: PingFangSC-Regular, PingFang SC;
        font-weight: 400;
        color: #FFFFFF;
        line-height: 40rpx;
        padding-left: 10rpx;
      }
      .goods-name {
        width: 168rpx;
        margin: 0 auto;
        text-align: center;
        font-size: 20rpx;
        font-family: PingFangSC-Medium, PingFang SC;
        font-weight: 500;
        color: #171A20;
        line-height: 28rpx;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
      }
    }
  }
}

.record-view {
  .title {
    padding: 0 30rpx;
    margin: 30rpx 0 22rpx;
    .title-left {
      font-size: 34rpx;
      font-family: PingFangSC-Medium, PingFang SC;
      font-weight: 500;
      color: #171A20;
      line-height: 48rpx;
    }
    .title-right {
      font-size: 24rpx;
      font-family: PingFangSC-Regular, PingFang SC;
      font-weight: 400;
      color: #7A808D;
      line-height: 34rpx;
      .rule-box {
        width: 24rpx;
        height: 24rpx;
        margin-left: 6rpx;
      }
    }
  }
  .record-list-view {
    .record-item {
      margin: 20rpx auto 0;
      width: 690rpx;
      height: 180rpx;
      background: #FFFFFF;
      border-radius: 16rpx;
      padding: 12rpx 0 10rpx 18rpx;
      .goods-img-view {
        width: 158rpx;
        height: 158rpx;
      }
      .record-item-right {
        flex: 1;
        margin-left: 22rpx;
        line-height: 42rpx;
        .goods-name-view {
          justify-content: flex-start;
          font-size: 24rpx;
          font-family: PingFangSC-Medium, PingFang SC;
          font-weight: 500;
          color: #171A20;
          .username, .goods-name {
            color: #0091FF;
          }
          .goods-name {
            display: inline-block;
            width: 200rpx;
            overflow: hidden;
            text-overflow:ellipsis;
            white-space: nowrap;
          }
          .txt {
            display: inline-block;
            margin: 0 8rpx;
          }
        }
        .goods-price-view {
          font-size: 24rpx;
          font-family: PingFangSC-Regular, PingFang SC;
          font-weight: 400;
          color: #FF495D;
        }
        .goods-time-view {
          font-size: 24rpx;
          font-family: PingFangSC-Regular, PingFang SC;
          font-weight: 400;
          color: #7A808D;
        }
      }
    }
  }
}

.regulation {
  .record-item {
    margin: 20rpx auto 0;
    width: 616rpx;
    height: 180rpx;
    background: #FFFFFF;
    border-radius: 16rpx;
    // padding: 12rpx 0 10rpx 18rpx;
    .goods-img-view {
      width: 158rpx;
      height: 158rpx;
    }
    .record-item-right {
      flex: 1;
      margin-left: 22rpx;
      line-height: 42rpx;
      .goods-name-view {
        justify-content: flex-start;
        font-size: 24rpx;
        font-family: PingFangSC-Medium, PingFang SC;
        font-weight: 500;
        color: #171A20;
        .username, .goods-name {
          color: #0091FF;
        }
        .goods-name {
          display: inline-block;
          width: 150rpx;
          overflow: hidden;
          text-overflow:ellipsis;
          white-space: nowrap;
        }
        .txt {
          display: inline-block;
          margin: 0 8rpx;
        }
      }
      .goods-price-view {
        font-size: 24rpx;
        font-family: PingFangSC-Regular, PingFang SC;
        font-weight: 400;
        color: #FF495D;
      }
      .goods-time-view {
        font-size: 24rpx;
        font-family: PingFangSC-Regular, PingFang SC;
        font-weight: 400;
        color: #7A808D;
      }
    }
  }
}

.buy-rule-view {
  padding-bottom: 46rpx;
  .title {
    padding: 0 30rpx;
    margin: 30rpx 0 22rpx;
    .title-left {
      font-size: 34rpx;
      font-family: PingFangSC-Medium, PingFang SC;
      font-weight: 500;
      color: #171A20;
      line-height: 48rpx;
    }
  }
  
  .box-view {
    width: 690rpx;
    margin: 0 auto;
    background: #FFFFFF;
    border-radius: 16rpx;
    padding: 28rpx 0;
    .rule-item {
      .rule-title {
        padding: 0 10rpx;
        font-size: 28rpx;
        font-family: PingFangSC-Medium, PingFang SC;
        font-weight: 500;
        color: #2E333F;
      }
      .rule-html {
        padding: 0 26rpx;
        font-size: 24rpx;
        font-family: PingFangSC-Regular, PingFang SC;
        font-weight: 400;
        color: #7A808D;
        line-height: 40rpx;
        margin: 12rpx 0 24rpx;
      }
    }
  }
}
</style>
