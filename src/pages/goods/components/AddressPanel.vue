// AddressPanel.vue
<script setup lang="ts">
import { onShow } from '@dcloudio/uni-app'
import { ref } from 'vue'
import type { AddressItem } from '@/types/address'
import { useMemberStore } from '@/stores'
import { useAddressStore } from '@/stores/modules/address'
const memberStore = useMemberStore()

// 点击关闭弹出层
const emit = defineEmits<{
  (event: 'close'): void
}>()
const jumpAddress = () => {
  uni.navigateTo({ url: '/pagesMember/address/address' })
}
const flag = ref(0)
const items = ref<any>('请选择收货地址')
const props = defineProps<{
  addressList: AddressItem[]
}>()

const addressStore = useAddressStore()
const iconChecked = (i = 0) => {
  if (addressState.value) {
    flag.value = i
    items.value = props.addressList[i].fullLocation
    addressStore.changeSelectedAddress(props.addressList[i])
    emit('close')
  }
}
const addressState = ref(false)
onShow(() => {
  if (memberStore.profile) return (addressState.value = true)
})
defineExpose({
  items,
  iconChecked,
})
</script>

<template>
  <view class="address-panel">
    <!-- 关闭按钮 -->
    <text class="close icon-close" @tap="emit('close')"></text>
    <!-- 标题 -->
    <view class="title">配送至</view>
    <!-- 内容 -->
    <view class="content" v-if="addressState">
      <view
        class="item"
        v-for="(item, i) in props.addressList"
        :key="item.id"
        @tap="iconChecked(i)"
      >
        <view class="user">{{ item.receiver }}{{ item.contact }}</view>
        <view class="address">{{ item.fullLocation }}</view>
        <text class="icon icon-checked" :class="flag === i ? 'icon-checked1' : ''"></text>
      </view>
    </view>
    <view class="footer">
      <view class="button primary" @tap="jumpAddress"> 地址管理 </view>
    </view>
  </view>
</template>

<style lang="scss">
.address-panel {
  padding: 0 30rpx;
  border-radius: 10rpx 10rpx 0 0;
  margin-bottom: 80rpx;
  position: relative;
  background-color: #fff;
}

.title {
  line-height: 1;
  padding: 40rpx 0;
  text-align: center;
  font-size: 32rpx;
  font-weight: normal;
  border-bottom: 1rpx solid #ddd;
  color: #444;
}

.close {
  position: absolute;
  right: 24rpx;
  top: 24rpx;
}

.content {
  max-height: 540rpx;
  overflow: auto;
  padding: 20rpx;

  .item {
    padding: 30rpx 50rpx 30rpx 60rpx;
    background-size: 40rpx;
    background-repeat: no-repeat;
    background-position: 0 center;
    background-image: url(https://pcapi-xiaotuxian-front-devtest.itheima.net/miniapp/images/locate.png);
    position: relative;
  }

  .icon {
    color: #999;
    font-size: 40rpx;
    transform: translateY(-50%);
    position: absolute;
    top: 50%;
    right: 0;
  }

  .icon-checked {
    color: #999;
  }

  .icon-checked1 {
    color: #27ba9b;
  }

  .icon-ring {
    color: #444;
  }

  .user {
    font-size: 28rpx;
    color: #444;
    font-weight: 500;
  }

  .address {
    font-size: 26rpx;
    color: #666;
  }
}

.footer {
  display: flex;
  justify-content: space-between;
  padding: 20rpx 0 40rpx;
  font-size: 28rpx;
  color: #444;

  .button {
    flex: 1;
    height: 72rpx;
    text-align: center;
    line-height: 72rpx;
    margin: 0 20rpx;
    color: #fff;
    border-radius: 72rpx;
  }

  .primary {
    color: #fff;
    background-color: #27ba9b;
  }

  .secondary {
    background-color: #ffa868;
  }
}
</style>
