<template>
  <view>
    <!-- 开盒记录start -->
    <view class="record-view">
      <view class="record-list-view">
        <view class="record-item flex" v-for="(item, index) in vgoods" :key="index">
          <view class="goods-img-view">
            <image :src="item.image" mode="aspectFit"></image>
          </view>
          <view class="record-item-right">
            <view class="goods-name-view flex">
              <text class="username">{{item.username}}</text>
              <text class="txt">开出了</text>
              <text class="goods-name">{{item.goods_name}}</text>
            </view>
            <view class="goods-price-view">
              参考价 ￥{{item.price}}
            </view>
            <view class="goods-time-view">
              开盒时间：{{item.createtime}}
            </view>
          </view>
        </view>
      </view>
    </view>
    <!-- 开盒记录end -->
  </view>
</template>

<script>
  export default {
    data() {
      return {
        vgoods: [],
        param:{
          page:1,
          limit:10
        },
        is_bottom:false
      }
    },
    onLoad () {
      this.getVgoods()
    },
    methods: {
      getVgoods(){
        this.$api.getVgoods(this.param).then(res=>{
        	if (res.code === 1) {
            if(res.data.list.length < this.param.limit) this.is_bottom = true;
        		this.vgoods = res.data.list
        	}
        })
      },
    },
    onReachBottom () {
    	if (!this.is_bottom) {
    		this.param.page++
    		this.getVgoods()
    	}
    }
  }
</script>

<style lang="scss" scoped>
.record-view {
  padding: 40rpx 0;
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
      margin: 0rpx auto 20rpx;
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

</style>
