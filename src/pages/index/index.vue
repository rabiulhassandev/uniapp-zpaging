<template>
  <view class="content">
    <image class="logo" src="/static/logo.png" />
    <view class="text-area">
      <text class="title">{{ title }}</text>
    </view>
    <view>
      <button @click="goToBasicList" class="btn">Default API Call</button>
      <button @click="goToZPagingApiCall" class="btn">Z-paging API Call</button>
    </view> 
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'
const title = ref('Hello')

// Navigate to the basic list page
const goToBasicList = () => {
  uni.navigateTo({
    url: '/pages/default-api-call/index'
  })
}

// Navigate to the Z-paging API Call page
const goToZPagingApiCall = () => {
  uni.navigateTo({
    url: '/pages/z-pagging-api-call/index'
  })
}

</script>

<style>
.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.logo {
  height: 200rpx;
  width: 200rpx;
  margin-top: 200rpx;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 50rpx;
}

.text-area {
  display: flex;
  justify-content: center;
}

.title {
  font-size: 36rpx;
  color: #8f8f94;
}
.btn {
  width: 300rpx;
  height: 80rpx;
  background-color: #6f00ff;
  color: white;
  border-radius: 40rpx;
  margin-top: 20rpx;
  text-align: center;
  line-height: 80rpx;
}

</style>
