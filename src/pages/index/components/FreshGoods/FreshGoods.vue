<script setup lang="ts">
import { ref } from 'vue'
import { onLoad } from '@dcloudio/uni-app';
import { getFreshGoodsAPI } from '@/services/home'

const freshGoods = ref<any>([])
const getFreshGoods = async (a: number) => {
  const res = await getFreshGoodsAPI(a)
  freshGoods.value = res.result
}
onLoad(() => {
  getFreshGoods(Math.floor(Math.random() * 3 + 1) * 3)
})
defineExpose({
  getFreshGoods
})
</script>

<template>
  <!-- 推荐专区 -->
  <view class="fresh">
    <text class="text">新鲜好物</text>
    <view class="fresh-view">
      <navigator hover-class="none" :url="`/pages/goods/goods?id=${item.id}`" class="item" v-for="item in freshGoods" :key="item.id">
        <image :src="item.picture" mode="scaleToFill" :title="item.desc"></image>
        <text>{{ item.name }}</text>
        <text style="color: red;">￥{{ item.price }} 元</text>
      </navigator>
    </view>

  </view>
</template>

<style lang="scss">
/* 热门推荐 */
.fresh {
  margin: 30rpx 10rpx 0;
  background-color: #fff;
  border-radius: 10rpx;

  .text {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 10rpx 28rpx 0 30rpx;

    &::before,
    &::after {
      content: '';
      width: 20rpx;
      height: 20rpx;
      background-image: url(@/static/images/bubble.png);
      background-size: contain;
      margin: 0 10rpx;
    }
  }

  .fresh-view {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    align-items: center;
    font-size: 20rpx;
    padding: 10rpx 0;

    .item {
      flex: 1;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      width: 210rpx;
      margin: 5rpx 8rpx;
      padding: 10rpx 8rpx 5rpx;
      border-radius: 10rpx;
      background-color: #dfe4ea;

      text {
        display: block;
        width: 210rpx;
        white-space: nowrap;
        text-overflow: ellipsis;
        overflow: hidden;
        text-align: left;
      }

      image {
        width: 210rpx;
        height: 210rpx;
        border-radius: 10rpx;
        margin-bottom: 5rpx;
      }
    }
  }
}
</style>

