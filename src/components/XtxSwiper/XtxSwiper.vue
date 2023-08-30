<script setup lang="ts">
// 断言数据类型
import type { BannerItem } from '@/types/home'

import { ref } from 'vue'
import { onLoad } from '@dcloudio/uni-app';
import { getHomeBannerAPI } from "@/services/home"


const props = defineProps<{ num: number }>()
const activeIndex = ref(0)
// ! 非空断言主观上排除掉空值情况
const onChange = (e: any) => activeIndex.value = e.detail!.current

const homeBanner = ref<BannerItem[]>([])
const getHomeBanner = async () => {
  const res = await getHomeBannerAPI(props?.num)
  homeBanner.value = res.result
}
onLoad(() => {
  getHomeBanner()
})
defineExpose({
  getHomeBanner
})
</script>

<template>
  <view class="carousel">
    <swiper :circular="true" :autoplay="false" :interval="3000" @change="onChange">
      <swiper-item v-for="item in homeBanner" :key="item.id">
        <navigator url="/pages/index/index" hover-class="none" class="navigator">
          <image mode="aspectFill" class="image" :src="item.imgUrl"></image>
        </navigator>
      </swiper-item>
    </swiper>
    <!-- 指示点 -->
    <view class="indicator">
      <text v-for="(item, index) in homeBanner" :key="item.id" class="dot"
        :class="{ active: index === activeIndex }"></text>
    </view>
  </view>
</template>

<style  lang="scss">
/* 轮播图 */
.carousel {
  height: 400rpx;
  position: relative;
  overflow: hidden;
  transform: translateY(0);
  background-color: #efefef;

  .indicator {
    position: absolute;
    left: 0;
    right: 0;
    bottom: 16rpx;
    display: flex;
    justify-content: center;

    .dot {
      width: 30rpx;
      height: 6rpx;
      margin: 0 8rpx;
      border-radius: 6rpx;
      background-color: rgba(255, 255, 255, 0.4);
    }

    .active {
      background-color: #fff;
    }
  }

  .navigator,
  .image {
    width: 100%;
    height: 100%;
  }
}
</style>
