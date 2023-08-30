<script setup lang="ts">
import CustomNavbar from './components/CustomNavbar/CustomNavbar.vue'
import CategoryPanel from './components/CategoryPanel/CategoryPanel.vue'
import HotPanel from './components/HotPanel/HotPanel.vue'
import FreshGoods from './components/FreshGoods/FreshGoods.vue'
import { ref } from 'vue'
import type { XtxGuessInstance } from '@/types/component'

// 组件实例
const xtxGuess = ref<XtxGuessInstance>()
const onScrolltolower = () => {
  xtxGuess.value?.getHomeGoodsGuessLike()
}
// 下拉刷新
const isTriggered = ref(false)
const xtxSwiper = ref(),
  hotPanel = ref(),
  freshGoods = ref()
const onRefresherrefresh = async () => {
  isTriggered.value = true
  // 同时发起promise请求
  await Promise.all([
    xtxSwiper.value.getHomeBanner(),
    hotPanel.value.getHomeHot(),
    xtxGuess.value.dropdownRefresh(),
    freshGoods.value.getFreshGoods(Math.floor(Math.random() * 3 + 1) * 3),
  ])
  isTriggered.value = false
}
</script>

<template>
  <CustomNavbar />
  <scroll-view scroll-y refresher-enabled @refresherrefresh="onRefresherrefresh" :refresher-triggered="isTriggered"
    @scrolltolower="onScrolltolower">
    <XtxSwiper ref="xtxSwiper" />
    <CategoryPanel />
    <HotPanel ref="hotPanel" />
    <FreshGoods ref="freshGoods" />
    <XtxGuess ref="xtxGuess" />
  </scroll-view>
</template>

<style lang="scss">
page {
  display: flex;
  flex-direction: column;
  height: 100%;
  background-color: #f7f7f7;

  scroll-view {
    flex: 1;
  }
}
</style>
