<template>
  <h1>Hello Vue</h1>
  
  <div style="max-width: 350px; margin: 0 auto; border: 1px solid #ccc; padding: 5px;">
    <div v-if="loading">Loading...</div>
    <div v-else>
      <div v-for="post in postList" :key="post.id" class="post-item">
        {{ post.title }}
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const postList = ref([]);
const loading = ref(true);

async function getPostList() {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/posts');
    postList.value = await response.json();
  } catch (error) {
    console.error('Error fetching post list:', error);
    postList.value = [];
  } finally {
    loading.value = false;
  }
}

// Call the function when component mounts
onMounted(() => {
  getPostList();
});
</script>

<style>
.post-item {
  padding: 20px;
  border-bottom: 1px solid #eee;
}
</style>