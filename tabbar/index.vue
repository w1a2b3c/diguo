<template>
	<view class="home">
		<audio id="audio" src="/h5/pagesA/static/bj1.mp3" loop></audio>
		<!--S 头部搜索 -->
		<view class="home_bar">
			<view class="home_bar_nav"></view>
			<view class="home_bar_con flex">
				<view class="home_bar_logo">
					<image :src="logo" mode="aspectFill"></image>
				</view>
				<view class="home_bar_search flexs" @click="goSearch">
					<image src="/static/image/home/sousuo@2x.png" mode="" style="background-size: 100%;"></image>
					<input type="text" disabled placeholder="拆个盒子" placeholder-style="color:#808080" />
				</view>
				<view class="home_bar_serve" @click="goServe">
					<image src="/static/image/home/kefu@2x.png" mode=""></image>
				</view>
			</view>
		</view>
		<!--E 头部搜索 -->
		<!--S 轮播图 -->
		<swiper autoplay circular class="swiper">
			<swiper-item v-for="(item,index) in bannerList" :key="'list-1-' + index">
				<image src="/static/image/tabbar/index-banner1.jpg" mode="aspectFill"></image>
			</swiper-item>
			<swiper-item v-for="(item,index) in bannerList" :key="'list-2-' + index">
				<image src="/static/image/tabbar/index-banner2.jpg" mode="aspectFill"></image>
			</swiper-item>
			<swiper-item v-for="(item,index) in bannerList" :key="'list-3-' + index">
				<image src="/static/image/tabbar/index-banner3.jpg" mode="aspectFill"></image>
			</swiper-item>
		</swiper>
		<view class="home_swiper">
			<!-- 菜单start -->
				<view class="home_swiper_nav flex">
					<view class="home_swiper_nav_li center" v-for="(item,index) in navList" :key="'list-4-' + index" @click="changeNav(item)">
						<image :src="item.image" mode=""></image>
						<text>{{ item.name }}</text>
					</view>
				</view>
		<!-- 菜单end -->
		<!-- 获奖公告1start -->
		<view class="award-notice" @tap="tomore">
		  <view class="title-view flex">
		    <view class="title-box flex">
		      <!-- <text class="title">获奖公告1</text> -->
		      <view class="title">
		        <image src="../../static/image/home/notice@2x.png" mode="aspectFit"></image>
		      </view>
		      <!-- <text class="tip">{{total}}人获得心仪商品</text> -->
		      <text class="tip"></text>
		    </view>
		    <view class="more flex">查看更多<image class="more-icon" src="../../static/image/publice/return@2x.png" mode="aspectFit"></image></view>
		  </view>
		  <swiper autoplay :display-multiple-items="4" circular class="goods-list flex" :class="{'goods-list-no': vgoods.length<4}">
		  	<swiper-item class="goods-item-swiper" v-for="item in vgoods">
		      <view class="goods-item">
		        <view class="goods-info">
		          <view class="goods-img">
		            <image :src="item.image" mode="aspectFit"></image>
		          </view>
		          <view class="goods-price flex">
		            ￥ {{item.price}}
		          </view>
		        </view>
		        <view class="bottom-view">
		          <view class="player-view flex">
		            <view class="player-avatar">
		              <image :src="item.avatar" mode="aspectFit"></image>
		            </view>
		            <text class="player-name">{{item.username}}</text>
		          </view>
		          <view class="goods-name">
		            {{item.goods_name}}
		          </view>
		        </view>
		      </view>
		  	</swiper-item>
		    <swiper-item class="goods-item-swiper" v-for="item in vgoods2">
		    </swiper-item>
		  </swiper>
		  <!-- <view class="goods-list flex">
		    <view class="goods-item" v-for="item in 10">
		      <view class="goods-info">
		        <image class="goods-img" src="../../static/image/open/erji@2x.png" mode="aspectFit"></image>
		        <view class="goods-price flex">
		          ￥ 480.00
		        </view>
		      </view>
		      <view class="bottom-view">
		        <view class="player-view flex">
		          <image class="player-avatar" src="../../static/image/publice/weixin@2x.png" mode="aspectFit"></image>
		          <text class="player-name">**玩家</text>
		        </view>
		        <view class="goods-name">
		          Apple/iPhoneiPhoneiPhoneiPhone
		        </view>
		      </view>
		    </view>
		  </view> -->
		  <view class="home_swiper_ul flex">
		  	<view class="home_swiper_li flexs" v-for="(item,index) in iconList" :key="'list-3-' + index">
		  		<image :src="item.image" mode=""></image>
		  		<text>{{ item.name }}</text>
		  	</view>
		  </view>
		</view>
		<!-- 获奖公告end -->
      <!-- 轮播图start -->
      <!-- <swiper autoplay circular class="swiper">
      	<swiper-item v-for="(item,index) in bannerList" :key="index" @click="changeBanner(item)">
      		<image :src="item.image" mode="aspectFill"></image>
      	</swiper-item>
      </swiper> -->
      <!-- 轮播end -->
		</view>
		<!--E 轮播图  -->
		<!-- S 试一试 -->
		<view class="home_trial flex">
			<view class="home_trial_img flexs">
		 <text>试试手气</text>
				<image src="/static/image/home/zuijia@2x.png" mode=""></image>
			</view>
			<view class="home_trial_go" @click="shishisq">
				<image src="/static/image/home/jinru@2x.png" mode=""></image>
			</view>
		</view>
		<!-- E 试一试 -->
		<!-- S 最新开箱 -->
		<view class="home_newest">
		<!-- 	<view class="home_newest_head flex">
				<view class="home_newest_head_new">最新开箱</view>
				<view class="home_newest_head_go flexs" @click="goNewBox">
					<text>查看更多</text>
					<image src="/static/image/publice/jinruer@2x.png" mode=""></image>
				</view>
			</view>
			<scroll-view scroll-x="true" class="scroll-view">
				<view class="scroll-view_li" v-for="(item,index) in newBoxList" :key="'list-5-' + index" @click="goMessage(item,0)">
					<view class="scroll-view_li_img">
						<image :src="item.goods_images[0]" mode="aspectFill"></image>
					</view>
					<view class="scroll-view_li_price flexs">
						<text>￥</text>
						<text>{{ item.coin_price }}</text>
					</view>
				</view>
			</scroll-view> -->
<!-- 			<view class="home_newest_ul flex">
				<view class="home_newest_li flexs" v-for="(item,index) in hotList" :key="index" @click="changeHot(item)">
					<view class="home_newest_img">
						<image :src="item.image" mode=""></image>
					</view>
					<view class="home_newest_con">
						<view class="home_newest_name">{{ item.name }}</view>
						<view class="home_newest_title">{{ item.title }}</view>
					</view>
				</view>
			</view> -->
		</view>
		<view class="home_swiper">
		<!-- E 最新开箱 -->
<!-- 		<swiper autoplay circular class="swiper">
			<swiper-item v-for="(item,index) in bannerList" :key="index" @click="changeBanner(item)">
				<image :src="item.image" mode="aspectFill"></image>
			</swiper-item>
		</swiper> -->
		</view>
		<!-- 轮播end -->
		<!-- S 推荐盲盒 -->
		<view class="home_recommend home_newest">
			<view class="home_newest_head flex">
				<view class="home_newest_head_new">推荐<text class="dot"></text></view>
				<view class="home_newest_head_go flexs" @click="goRecommend">
					<text>更多分类</text>
					<image src="/static/image/publice/jinruer@2x.png" mode=""></image>
				</view>
			</view>
			<view class="home_recommend_ul flex ">
<!--       <view class="home_recommend_item flex" v-for="(item,index) in shopList" :key="index" @click="goMessage(item,0)">
          <view class="item-box">
            <view class="coin-view">
              <text class="coin-price">{{item.coin_price}}</text>
              <text class="coin-unit">金币/抽</text>
            </view>
            <text class="box-name">{{item.box_name}}</text>
            <view class="goods-list flex">
              <view class="goods-img goods-img-0">
                <image :src="item.goods_images[0]" mode="aspectFit"></image>
              </view>
              
              <swiper :autoplay="false" :display-multiple-items="4" class="goods-list4 flex">
              	<swiper-item class="goods-img" v-for="(img, img_index) in item.goods_images.slice(1)" :key="img_index">
                  <image :src="img" mode="aspectFit"></image>
                </swiper-item>
                <swiper-item class="goods-img" v-for="t in item.goods_images2"></swiper-item>
              </swiper>

            </view>
            <view class="bottom-view flex">
              <text class="goods-num">共{{item.goods_num}}款商品</text>
              <view class="goods-jiazhi">
                <view class="min-max">
                  <text class="price">{{item.price_min}}~{{item.price_max}}</text>
                </view>
              </view>
            </view>
          </view>
        </view> -->
		<view class="home_recommend_li" v-for="(item,index) in shopList" :key="'list-6-' + index" @click="goMessage(item,0)">
					<view class="home_recommend_li_img">
						<view class="home_recommend_li_img_top flex">
							<view class="home_recommend_li_img_box">
								<image :src="item.goods_images[0]" mode="aspectFit"></image>
							</view>
							<!-- <view class="home_recommend_li_img_right" v-if="item.right.length != 0">
								<image :src="items" mode="aspectFit" v-for="(items,index) in item.right" :key="index"></image>
							</view> -->
						</view>
						<!-- <view class="home_recommend_li_img_bot">
							<image :src="items" mode="aspectFit" v-for="(items,index) in  item.bot" :key="index"></image>
						</view> -->
					</view>
					<view class="home_recommend_li_main">
						<view class="home_recommend_li_price">
							<view class="li_price">
								<view class="price">{{ item.price_min }}~{{ item.price_max }}</view>
							</view>
						</view>
						<view class="home_recommend_li_name">{{ item.box_name }}</view>
						<view class="home_recommend_li_glod flex">
							<view class="item-box">
								<text class="coin-price">{{ item.coin_price }}</text>
								<text class="coin-unit1">金币/抽</text>
							</view>
								<text class="gjksp">共{{ item.goods_num }}款商品</text>
						</view>
					</view>
				</view>
			
      </view>
		</view>
		<!-- E 推荐盲盒 -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
        vgoods: [],
        vgoods2: 0,
        total:0,
				iconList:[
					{
						image:'/static/image/home/zhengpinbaozheng@2x.png',
						name:'官方正品保证'
					},
					{
						image:'/static/image/home/yijianhuishou@2x.png',
						name:'一键秒回收'
					},
					{
						image:'/static/image/home/youhui@2x.png',
						name:'全网最优惠'
					}
				],
				navList:[
					{
						image:'/static/image/home/chaowanmanghe@2x.png?i=1',
						name:'欢乐拆盒',
						url:'/pages/index/more'
					},
					{
						image:'/static/image/home/youhuichongzhi@2x.png?i=1',
						name:'优惠充值',
						url:'/pages/me/recharge'
					},
					{
						image:'/static/image/home/fahuoliucheng@2x.png?i=1',
						name:'发货流程',
						url:'/pages/index/flow'
					},
					{
						image:'/static/image/home/xinshoubangzhu@2x.png?i=1',
						name:'新手帮助',
						url:'/pages/index/help'
					},
				],//导航列表
				hotList:[
					{
						image:'/static/image/home/remen@2x.png',
						name:'热门专区',
						title:'跟随热门手气更欧',
						url:'/pages/index/hot?type=0'
					},
					{
						image:'/static/image/home/dijia@2x.png',
						name:'低价专区',
						title:'低价试试手气',
						url:'/pages/index/hot?type=1'
					}
				],
				bannerList:[],//轮播图
				logo:'',//logo
				shopList:[],//获取推荐列表
				newBoxList:[],//最新开箱
				pages:0,
				page:1,//
			};
		},
		methods:{
      sumUv(){
        this.$api.sumuv().then(res=>{
          
        })
      },
      tomore () {
        uni.navigateTo({
          url: '/pages/index/records'
        })
      },
      getVgoods(){
        this.$api.getVgoods().then(res=>{
        	if (res.code === 1) {
        		this.vgoods = res.data.list
        		this.total = res.data.total
            if (this.total < 4) {
              this.vgoods2 = 4 - this.total
            }
        	}
        })
      },
			//去客服
			goServe () {
				uni.navigateTo({url:'/pagesB/pages/server'})
			},
			//试试手气
			shishisq () {
				uni.navigateTo({url:'/pages/index/details?id=' + 25 + '&type=0'})
			},
			//去推荐，盲盒
			goRecommend () {
				uni.navigateTo({url:'/pages/index/recommend'})
			},
			//最新开箱
			goNewBox () {
				uni.navigateTo({url:'/pages/index/newBox'})
			},
			//轮播图
			
			changeBanner (item) {
				if (item.type == "word") {
					uni.setStorageSync('bannerMessage',JSON.stringify(item.value))
					uni.navigateTo({url:'/pagesA/pages/bannerMessage'})
				} else if (item.type == 'link') {
					location.href = item.value
				} else if (item.type == 'box'){
					uni.navigateTo({url:'/pages/index/details?id=' + 36 + '&type=0'})
				}
			},
			//去搜索
			goSearch () {
				uni.navigateTo({url:'/pages/index/search'})
			},
			//去详情
			goMessage (item,type) {
				uni.navigateTo({url:'/pages/index/details?id=' + item.box_id + '&type=' + type})
			},
			//热门专区
			changeHot (item) {
				uni.navigateTo({url:item.url})
			},
			//头部四个
			changeNav (item) {
				uni.navigateTo({url:item.url})
			},
			//获取轮播图
			getbannerList () {
				
			},
			//最新开箱
			getNewbox () {
				this.$api.newestOpen().then(res=>{
					if (res.code === 1) {
						this.newBoxList = res.data
					}
				})
			},
			//获取推荐列表
			getShopList () {
				this.$api.recommend().then(res=>{
					if (res.code === 1) {
						res.data.data.forEach(item=>{
							item.image = item.goods_images[0]
							item.right = []
							item.bot = []
							item.goods_images.forEach((em,index)=>{
								if (index > 0 && index < 3) {
									item.right.push(em)
								}
								if (index > 2) {
									item.bot.push(em)
								}
							})
              if (item.goods_images.length < 5) {
                item.goods_images2 = 5 - item.goods_images.length
              }
						})
						this.shopList = this.page == 1 ? res.data.data : [...this.shopList,...res.data.data],
            this.pages = res.data.last_page
					}
				})
			},
			//获取首页信息
			getBaseInfo () {
				this.$api.baseInfo().then(res=>{
					if (res.code === 1) {
						this.logo = res.data.logo
						this.bannerList = res.data.banner
					}
				})
			}
		},
		onLoad({sharecode,token,is_channel}) {
			if (sharecode) {
				uni.setStorageSync('sharecode',sharecode)
			}
      if (is_channel) {
      	uni.setStorageSync('is_channel',is_channel)
      }
			if (token) {
				uni.setStorageSync('token',token)
			}
			this.getBaseInfo()
			this.getbannerList()
			this.getNewbox()
			this.getShopList()
      this.getVgoods()
      this.sumUv();
		},
    onShow () {
      let is_notice = uni.getStorageSync('is_notice')
      uni.removeStorageSync('is_notice');
      if (is_notice == 1) {
        setTimeout(() => {
          uni.showModal({
            title: '提示',
            content: '恭喜，获得10金币',
            success: function (res) {
              if (res.confirm) {
                  console.log('用户点击确定');
              } else if (res.cancel) {
                  console.log('用户点击取消');
              }
            }
          })
        }, 1000)
      }
    },
	mounted(){
		let audio = document.getElementById("audio").querySelector("audio")
		audio.play()
	},
		onReachBottom () {
			if (this.page < this.pages) {
				this.page++
				this.getShopList()
			}
		}
	}
	
</script>

<style lang="scss">
page {
  background-color: #FFFFFF;
}
//主页双产品
.home_recommend_ul .home_recommend_li .home_recommend_li_img .home_recommend_li_img_box uni-image{
	width: 145px!important;
}
.home_recommend_li_price .li_price {
	text-align: left;
	width: 83px;
	height: 19px;
	line-height: 19px;
	position: absolute;
	right: 0;
	top: 0;
}

.home_recommend_ul .home_recommend_li .coin-price{
	    font-size: 18px;
	    font-family: DINAlternate-Bold, DINAlternate;
	    font-weight: bold;
	    color: #F55D22!important;
	    line-height: 26px;
}
.home_recommend_ul .home_recommend_li .coin-unit1{
    font-size: 11px;
    font-family: PingFangSC-Regular, PingFang SC;
    font-weight: bold!important;
    color: #FA6400!important;
    line-height: 16px;
    margin-left: 3px;
	
}
.home_recommend_ul .home_recommend_li .gjksp{
	height: 16px;
	line-height: 25px;
}
.home_recommend_li_price .li_price .price{
	font-size: 13px;
	font-family: DINAlternate-Bold, DINAlternate;
	font-weight: bold;
	color: #FFFFFF;
	line-height: 19px;
}
.home_bar {
	width: 100%;
	position: sticky;
	top: 0;
	left: 0;
	padding: 0 30rpx;
	z-index: 2021;
	background: #FFFFFF;
	.home_bar_nav {width: 100%;}
	.home_bar_con {
		height: 88rpx;
	}
	.home_bar_logo {
		width: 66rpx;
		height: 66rpx;
	}
	.home_bar_search {
		width: 600rpx;
		height: 58rpx;
		padding: 0 20rpx;
		background: #F7F7F7;
		border-radius: 200rpx;
		image {
			width: 44rpx;
			height: 44rpx;
		}
		input {
			flex: 1;
			font-size: 26rpx;
			margin-left: 15rpx;
		}
	}
	.home_bar_serve {
		width: 44rpx;
		height: 44rpx;
	}
}
.home_swiper {
	padding: 0 30rpx;
	background: #FFFFFF;
  .award-notice {
    padding: 22rpx 15rpx 0 30rpx;
    // width: 686rpx;
    height: 358rpx;
    background: url(../../pagesA/static/jpgg.jpg);
	background-size: 100%;
    border-radius: 24rpx 24rpx 0rpx 0rpx;
    .title-view {
      padding-right: 24rpx;
      .title {
        font-size: 39rpx;
        font-family: YouSheBiaoTiHei;
        color: #FFFFFF;
        margin-right: 20rpx;
        width: 142rpx;
        height: 30rpx;
      }
      .tip {
        font-size: 24rpx;
        font-family: PingFangSC-Medium, PingFang SC;
        font-weight: 500;
        color: #FFFFFF;
        line-height: 34rpx;
      }
      .more {
        font-size: 24rpx;
        font-family: PingFangSC-Medium, PingFang SC;
        font-weight: 500;
        color: #FFFFFF;
        line-height: 34rpx;
        // margin-top: 12rpx;
        .more-icon {
          width: 24rpx;
          height: 24rpx;
        }
      }
    }

    .goods-list {
      margin: 20rpx 0 0;
      height: 206rpx;
      justify-content: flex-start;
      .goods-item {
        margin-right: 18rpx;
        width: 146rpx;
        height: 206rpx;
        background: #FFFFFF;
        border-radius: 8rpx;
        .goods-info {
          position: relative;
          height: 106rpx;
          .goods-img {
            width: 100%;
            height: 100%;
          }
          .goods-price {
            position: absolute;
            bottom: -10rpx;
            right: 0;
            z-index: 1;
            justify-content: center;
            min-width: 92rpx;
            height: 32rpx;
            background: linear-gradient(90deg, #FF495C 0%, #FF769E 100%);
            border-radius: 16rpx 0px 0px 0px;
            font-size: 18rpx;
            font-family: PingFangSC-Medium, PingFang SC;
            font-weight: 500;
            color: #FFFFFF;
            line-height: 26rpx;
          }
        }
        .bottom-view {
          padding: 20rpx 0 0 16rpx;
          width: 146rpx;
          height: 100rpx;
          background: #E8DBF9;
          border-radius: 0px 0px 8rpx 8rpx;
          .player-view {
            justify-content: flex-start;
            .player-avatar {
              width: 32rpx;
              height: 32rpx;
              border-radius: 50%;
            }
            .player-name{
              margin-left: 4rpx;
              font-size: 18rpx;
              font-family: PingFangSC-Medium, PingFang SC;
              font-weight: 500;
              color: #171A20;
              line-height: 26rpx;
            }
          }
          .goods-name {
            margin-top: 6rpx;
            width: 124rpx;
            font-size: 18rpx;
            font-family: PingFangSC-Medium, PingFang SC;
            font-weight: 500;
            color: #171A20;
            line-height: 26rpx;
            white-space: nowrap;
            text-overflow: ellipsis;
            overflow: hidden;
          }
        }
      }
    }
    .home_swiper_ul {
      padding-right: 24rpx;
      margin-top: 16rpx;
    }
  }
	.swiper {
		height: 156rpx;
    border-radius: 99rpx;
    overflow: hidden;
    overflow: hidden;
		image {
			border-radius: 20rpx;
		}
	}
	.home_swiper_li {
		height: 64rpx;
		image {
			width: 34rpx;
			height: 34rpx;
			margin-right: 10rpx;
		}
		text {
      color: #FFF;
		}
	}
  .home_swiper_nav {
    justify-content: space-around;
  }
	.home_swiper_nav_li {
		height: 182rpx;
		flex-direction: column;
		image {
			width: 70rpx;
			height: 70rpx;
			margin-bottom: 10rpx;
		}
		text {
			color: #000000;
			font-size: 26rpx;
		}
	}
}
.home_trial {
	height: 88rpx;
	margin: 10rpx 0;
	padding: 0 30rpx;
	background: #FFFFFF;
	.home_trial_img {
		text {
			font-size: 28rpx;
		}
		image {
			width: 38rpx;
			height: 38rpx;
			margin-left: 10rpx;
		}
	}
	.home_trial_go {
		width: 50rpx;
		height: 50rpx;
	}
}
.home_newest {
	background: #FFFFFF;
	.home_newest_head {
		padding: 30rpx 28rpx;
		.home_newest_head_new {
			font-size: 28rpx;
			font-weight: bold;
      position: relative;
      .dot {
        position: absolute;
        right: -16rpx;
        top: 0rpx;
        background-color: #fff;
        border: 6rpx solid #F7B500;
        border-radius: 50%;
      }
		}
		.home_newest_head_go {
			text {
				color: #999999;
			}
			image {
				width: 22rpx;
				height: 22rpx;
				margin-left: 10rpx;
			}
		}
	}
	.scroll-view {
		white-space:nowrap;
		box-sizing: border-box;
		padding-left: 28rpx;
		.scroll-view_li {
			margin-right: 20rpx;
			display: inline-block;
			.scroll-view_li_img {
				image{
					width: 150rpx;
					height: 150rpx;
					border-radius: 10rpx;
				}
				margin-bottom: 20rpx;
			}
			.scroll-view_li_price {
				text {
					color: #CF271B;
					&:last-child {
						font-size: 30rpx;
					}
				}
			}
		}
	}
	.home_newest_ul {
		padding: 4rpx 28rpx 4rpx 28rpx;
		background-color: #F8FAFB;
		.home_newest_li {
			width: 332rpx;
			height: 130rpx;
			padding-left: 30rpx;
			background: #F8FAFB;
			border-radius: 6rpx;
			.home_newest_img {
				width: 70rpx;
				height: 70rpx;
				margin-right: 24rpx;
			}
			.home_newest_name {
				font-size: 30rpx;
				font-weight: bold;
			}
			.home_newest_title {
				color: #999999;
				margin-top: 10rpx;
			}
		}
	}
}
.home_recommend_ul {
	flex-wrap: wrap;
  justify-content: center;
	// padding: 0 28rpx;
  .home_recommend_item {
    margin-top: -30rpx;
    justify-content: center;
    width: 746rpx;
    height: 384rpx;
    background-image: url(../../static/image/home/bg@2x.png);
    background-size: 100% 100%;
    background-position: center;
    .item-box{
      width: 686rpx;
      height: 324rpx;
      padding: 22rpx 0 20rpx 18rpx;
      position: relative;
      .coin-view {
        position: absolute;
        top: 20rpx;
        right: 20rpx;
        .coin-price {
          font-size: 48rpx;
          font-family: DINAlternate-Bold, DINAlternate;
          font-weight: bold;
          color: #F55D22;
          line-height: 56rpx;
        }
        .coin-unit {
          font-size: 24rpx;
          font-family: PingFangSC-Regular, PingFang SC;
          font-weight: 400;
          color: #FA6400;
          line-height: 34rpx;
          margin-left: 8rpx;
        }
      }
      .box-name {
        font-size: 32rpx;
        font-family: PingFangSC-Medium, PingFang SC;
        font-weight: 500;
        color: #2E333F;
        line-height: 44rpx;
      }
      .goods-list {
        margin: 44rpx 0 6rpx;
        .goods-img {
          width: 92rpx;
          height: 92rpx;
        }
        .goods-img-0 {
          width: 148rpx;
          height: 148rpx;
        }
        .goods-list4 {
          flex: 1;
          height: 92rpx;
          justify-content: flex-start;
        }
      }
      .bottom-view {
        .goods-num {
          font-size: 24rpx;
          font-family: PingFangSC-Regular, PingFang SC;
          font-weight: 400;
          color: #7A808D;
          line-height: 34rpx;
          margin-left: 8rpx;
        }
        .goods-jiazhi {
          margin-right: 20rpx;
          background-image: url(../../static/image/home/jiazhi10@2x.png);
          background-size: 100% 100%;
          background-position: center;
          width: 274rpx;
          height: 40rpx;
          position: relative;
          .min-max {
            text-align: left;
            width: 162rpx;
            height: 40rpx;
            line-height: 40rpx;
            position: absolute;
            right: 0;
            top: 0;
            .unit {
              font-size: 20rpx;
              font-family: PingFangSC-Regular, PingFang SC;
              font-weight: 400;
              color: #FFFFFF;
              line-height: 28rpx;
            }
            .price {
              font-size: 28rpx;
              font-family: DINAlternate-Bold, DINAlternate;
              font-weight: bold;
              color: #FFFFFF;
              line-height: 32rpx;
            }
          }
        }
      }
    }
  }
	.home_recommend_li {
		width: 332rpx;
		padding: 20rpx;
		margin-bottom: 30rpx;
		background: url(../../static/image/tabbar/scpbj.png);
		background-size: 100%;
		box-shadow: 0rpx 0rpx 10rpx 0rpx rgba(153, 153, 153, 0.1);
		border-radius: 10rpx;
		margin: 5px 5px;;
		.home_recommend_li_img {
			height: auto;
			.home_recommend_li_img_top {
				align-items: flex-start;
			}
			.home_recommend_li_img_bot {
				display: flex;
				margin-top: 20rpx;
				justify-content: flex-end;
				image {
					width: 85rpx;
					height: 85rpx;
					margin-left: 20rpx;
					&:first-child {
						margin-left: 0;
					}
					border-radius: 6rpx;
				}
			}
			.home_recommend_li_img_box {
				image {
					width: 190rpx;
					height: 190rpx;
					border-radius: 6rpx;
				}
			}
			.home_recommend_li_img_right {
				image {
					width: 85rpx;
					height: 85rpx;
					border-radius: 6rpx;
					&:first-child {
						margin-bottom: 20rpx;
					}
				}
			}
		}
		.home_recommend_li_price {
			// width: 280rpx;
			height: 40rpx;
			color: #FFFFFF;
			margin: 20rpx 0;
			font-size: 26rpx;
			line-height: 40rpx;
			padding-left: 18rpx;
			background: url(/h5/static/img/jiazhi10@2x.27fb9543.png);
			background-size: 100% 100%;
			border-radius: 20rpx;
			position: relative;
		}
		.home_recommend_li_name {
			font-size: 28rpx;
			display: -webkit-box;
			-webkit-box-orient: vertical;
			-webkit-line-clamp: 2;
			overflow: hidden;
			height: 76rpx;
			line-height: 40rpx;
		}
		.home_recommend_li_glod {
			text {
				color: #CF271B;
				font-size: 26tpx;
				&:last-child {
					color: #666666;
					font-size: 24rpx;
				}
			}
		}
	}
}
</style>
