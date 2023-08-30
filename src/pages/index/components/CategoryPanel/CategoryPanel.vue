<script setup lang="ts">
// 断言数据类型
import type { CategoryItem } from '@/types/home'

import { ref } from 'vue'
import { onLoad } from '@dcloudio/uni-app';
import { getHomeCategoryAPI } from "@/services/home"

const activeIndex = ref(0)
// ! 非空断言主观上排除掉空值情况
const onChange = (e: any) => activeIndex.value = e.detail!.current


const categoryList = ref<CategoryItem[]>([])
const getHomeCategory = async () => {
  const res = await getHomeCategoryAPI()
  categoryList.value = res.result
}
onLoad(() => {
  getHomeCategory()
})
</script>

<template>
  <view class="carousel">
    <swiper @change="onChange">
      <swiper-item class="swiperItem">
        <navigator url="/pages/index/index" hover-class="none" class="navigator" v-for="item in categoryList.slice(0, 8)"
          :key="item.id">
          <image mode="aspectFill" class="image" :src="item.icon"></image>
          <text>{{ item.name }}</text>
        </navigator>
      </swiper-item>
      <swiper-item class="swiperItem">
        <navigator url="/pages/index/index" hover-class="none" class="navigator"
          v-for="item in categoryList.slice(8, categoryList.length)" :key="item.id">
          <image mode="aspectFill" class="image" :src="item.icon"></image>
          <text>{{ item.name }}</text>
        </navigator>
      </swiper-item>
    </swiper>
    <!-- 指示点 -->
    <view class="indicator">
      <text v-for="(item, index) in 2" :key="item" class="dot" :class="{ active: index === activeIndex }"></text>
    </view>
  </view>
</template>

<style  lang="scss">
/* 轮播图 */
.carousel {
  height: 360rpx;
  margin: 20rpx 0;
  position: relative;
  overflow: hidden;
  transform: translateY(0);
  background-color: #fff;

  .indicator {
    position: absolute;
    left: 0;
    right: 0;
    bottom: 10rpx;
    display: flex;
    justify-content: center;

    .dot {
      width: 35rpx;
      height: 8rpx;
      margin: 0 8rpx;
      border-radius: 8rpx;
      background-color: rgb(178, 190, 195,0.3);
    }

    .active {
      background-color: #b2bec3;
    }
  }

  .swiperItem,
  swiper {
    display: flex;
    flex-wrap: wrap;
    justify-content: start;
    height: 340rpx;


    .navigator {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      width: 185rpx;
      height: 170rpx;

      .image {
        width: 100rpx;
        height: 100rpx;
      }
    }
  }
}
</style>
