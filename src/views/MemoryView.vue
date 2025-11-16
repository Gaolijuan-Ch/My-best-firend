<template>
  <div class="memory-page">
    <h2 class="title">🎀 我们的大学回忆</h2>

    <van-swipe class="my-swipe" autoplay="3000" indicator-color="pink">
      <van-swipe-item >
      
      </van-swipe-item>
    </van-swipe>

    <div class="desc-card">
      <h3>✨永远记得的瞬间</h3>
      <p>
      </p>
    </div>

    <van-button type="primary" round class="back-btn" @click="goBack">
      返回首页
    </van-button>
  </div>
</template>

<script setup lang="ts">
import { useRouter } from 'vue-router'

const router = useRouter()

const goBack = () => {
  router.push('/')
}

// const photos = [
// ]
</script>

<style scoped>
.memory-page {
  min-height: 100vh;
  padding: 22px;
  background: linear-gradient(135deg, #ffeaf5, #ffd8f2);
}

.title {
  font-size: 22px;
  text-align: center;
  color: #ff5b9b;
  margin-bottom: 20px;
}

.my-swipe {
  height: 220px;
  border-radius: 18px;
  overflow: hidden;
}

.photo {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.desc-card {
  background: #fff;
  margin-top: 20px;
  padding: 20px;
  border-radius: 20px;
  color: #ff5b9b;
  font-size: 15px;
  box-shadow: 0 4px 14px rgba(255, 130, 170, 0.18);
}

.back-btn {
  width: 100%;
  margin-top: 24px;
  background: #ff7eb8 !important;
  border: none;
}
</style>
