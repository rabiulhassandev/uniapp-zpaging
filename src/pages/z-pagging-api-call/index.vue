<template>
  <view class="container">
    <z-paging
      ref="paging"
      v-model="postList"
      @query="getPostList"
      :fixed="false"
      :auto-show-back-to-top="true"
      :default-page-size="10"
    >
      <template #top>
        <view class="header">
          <h1>Post List</h1>
        </view>
      </template>
      
      <view 
        v-for="post in postList" 
        :key="post.id" 
        class="post-item"
      >
        <text class="post-title">{{ post.id }}. {{ post.title }}</text>
        <text class="post-body">{{ post.body }}</text>
      </view>
      
      <template #empty>
        <view class="empty-view">
          <text class="empty-text">No posts available</text>
          <button @click="refreshList" class="refresh-btn">Try Again</button>
        </view>
      </template>
      
      <template #loadingMoreNoMore>
        <view class="no-more-data">
          <text class="no-more-text">No more posts</text>
        </view>
      </template>
    </z-paging>
  </view>
</template>

<script setup>
import { toRaw , reactive, ref } from 'vue';
import { onLoad } from '@dcloudio/uni-app';
// Refs
const paging = ref();
const postList = ref([]);
let allPosts = [];

onLoad(() => {
  console.log("See Posts: ", toRaw(myPosts));
});


const myPosts = [
  // 100 posts
  { id: 1, title: 'Post 1', body: 'This is the body of post 1' },
  { id: 2, title: 'Post 2', body: 'This is the body of post 2' },
  { id: 3, title: 'Post 3', body: 'This is the body of post 3' },
  { id: 4, title: 'Post 4', body: 'This is the body of post 4' },
  { id: 5, title: 'Post 5', body: 'This is the body of post 5' }
] ;


// default paging value 



onLoad(() => {
  // getPostList(1, 10);
});


// Fetch all posts from API
async function fetchAllPosts() {
  try {
    const res = await new Promise((resolve, reject) => {
      uni.request({
        url: 'https://jsonplaceholder.typicode.com/posts',
        success: resolve,
        fail: reject
      });
    });
    
    if (res.statusCode !== 200) {
      throw new Error(`HTTP ${res.statusCode}`);
    }

    const data = res.data || [];
    console.log('Fetched posts:', data.length);
    
    allPosts = data;
    return true;
    
  } catch (error) {
    console.error('Fetch error:', error);
    uni.showToast({
      title: 'Failed to load posts',
      icon: 'none',
      duration: 2000
    });
    return false;
  }
}

// Handle pagination
async function getPostList(pageNo, pageSize) {
  

  paging.value.complete([
    { id: 1, title: 'post title', body: 'post body' }
  ])


  console.log(`Fetching page ${pageNo} with size ${pageSize}`);
  if (pageNo === 1) {
    // First load - fetch all data
    const success = await fetchAllPosts();
    if (!success) {
      paging.value.complete([]);
      return;
    }
  }
  
  // Simulate pagination on frontend
  const start = (pageNo - 1) * pageSize;
  const end = start + pageSize;

  const pageData = allPosts.slice(start, end);

  console.log("Slice Posts: ", pageData) /// 10
  // Complete with the paginated data
  paging.value.complete(pageData);
  console.log(postList.value, "Post List Data");
}

// Handle post click
function handlePostClick(postId) {
  uni.navigateTo({
    url: `/pages/post/detail?id=${postId}`
  });
}

// Manual refresh
function refreshList() {
  allPosts.value = [];
  paging.value.reload();
}
</script>

<style>
.container {
  flex: 1;
  background-color: #f5f5f5;
}

.header {
  padding: 20rpx;
  text-align: center;
  background-color: #fff;
}

.post-item {
  padding: 24rpx;
  margin: 20rpx;
  background-color: #fff;
  border-radius: 12rpx;
  box-shadow: 0 2rpx 6rpx rgba(0, 0, 0, 0.05);
}

.post-title {
  font-size: 30rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 12rpx;
  display: block;
}

.post-body {
  font-size: 26rpx;
  color: #666;
  display: block;
}

.empty-view {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 100rpx 0;
}

.empty-image {
  width: 200rpx;
  height: 200rpx;
  margin-bottom: 30rpx;
}

.empty-text {
  font-size: 28rpx;
  color: #999;
  margin-bottom: 20rpx;
}

.refresh-btn {
  margin-top: 30rpx;
  width: 200rpx;
}

.no-more-data {
  padding: 30rpx 0;
  text-align: center;
}

.no-more-text {
  font-size: 26rpx;
  color: #999;
}
</style>