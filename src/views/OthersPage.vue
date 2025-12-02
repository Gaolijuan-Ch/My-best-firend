<template>
  <div class="memory-page">
    <div class="title">
      <img src="../assets/hellokitty3.svg" alt="logo" style="width: 70px; height: 70px; margin: 4px;" @click="goBack" title="点我返回上一页"/>
    </div>
    <!-- 轮播 -->
    <!-- <van-swipe class="my-swipe" autoplay="3000" indicator-color="pink">
      <van-swipe-item> </van-swipe-item>
    </van-swipe> -->
    <div class="card">
      <h3> 新的一岁，要对你说的✨</h3>
      <p></p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { useRouter } from 'vue-router'

const router = useRouter()

const goBack = () => {
  router.push('/memory')
}

// const photos = [
// ]
</script>

<style scoped>
.memory-page {
  min-height: 100vh;
  padding: 22px;
  background-image: url(../assets/壁纸3.jpg);
  background-size: cover;
  background-position: center;
}

.card {
  height: 620px;
  text-align: center;
  background: #fff;
  margin-top: 20px;
  color: #070707;
  font-size: 15px;  
  border-radius: 24px;
  padding: 20px 20px;
  box-shadow: 0 8px 32px rgba(227, 184, 197, 0.15);
  text-align: center;
  position: relative;
 
}
.title {
  font-size: 22px;

  color: #cab1ba;
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



.back-btn {
  width: 100%;
  margin-top: 24px;
  background: #ff7eb8 !important;
  border: none;
}
</style>
