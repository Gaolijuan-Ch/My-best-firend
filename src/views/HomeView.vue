<template>
  <div class="birthday-container">
  

    <!-- 主卡片内容 -->
    <div class="card">
      <h1 class="title">
        🎂 生日快乐
        <br />
        我的好闺蜜！
      </h1>

      <p class="subtitle">
        今天要开心到飞起来！
        <br />
        愿我们的友谊永远亮闪闪 ✨
      </p>

      <!-- 图片展示区 -->
      <div class="image-container">
        <van-image
          round
          width="180px"
          height="180px"
          alt="生日蛋糕"
        />
      </div>

      <!-- 按钮区域 -->
      <van-button type="primary" round class="main-btn" @click="goMemory">
        💗 打开我们的大学回忆
      </van-button>
    </div>

    <!-- 底部装饰 -->
    <div class="footer-decor">
      <span>✨</span>
      <span>💖</span>
      <span>🎉</span>
      <span>💝</span>
      <span>✨</span>
    </div>
  </div>
</template>

<script setup lang="ts">
import { useRouter } from 'vue-router'
const router = useRouter()

// 跳转到回忆页面
const goMemory = () => {

  router.push('/memory')
}
</script>

<style scoped>
/* 页面基础样式 */
.birthday-container {
  min-height: 100vh;
  padding: 20px;
  background-image: url(../assets/壁纸4.jpg);
  background-size: cover;
  background-position: center;
  background-position: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  position: relative;
  overflow: hidden;
}


/* 主卡片样式 */
.card {
  width: 90%;
  max-width: 400px;
  
  transform: translateY(100px);
  background: rgba(255, 255, 255, 0.4); 
  /* 毛玻璃核心属性 */
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px); /* 兼容 Safari */
  border-radius: 24px;
  padding: 20px 20px;
  /* 调整阴影为更柔和的效果 */
  box-shadow: 0 8px 32px rgba(255, 100, 150, 0.15);
  text-align: center;
  margin-top: 140px;
  position: relative;
  /* 增加边框增强通透感（可选） */
  border: 1px solid rgba(255, 255, 255, 0.2);
}

/* 装饰元素 */
.decor {
  position: absolute;
  font-size: 24px;
}

.top-left {
  top: 12px;
  left: 16px;
}

.top-right {
  top: 12px;
  right: 16px;
}

.bottom-left {
  bottom: 12px;
  left: 16px;
}

.bottom-right {
  bottom: 12px;
  right: 16px;
}

/* 文本样式 */
.title {
  color: #ff5b9b;
  font-size: 28px;
  font-weight: 700;
  margin: 0 0 16px;
  line-height: 1.4;
}

.subtitle {
  color: #ff7db4;
  margin: 0 0 24px;
  font-size: 16px;
  line-height: 1.6;
}

/* 图片容器 */
.image-container {
  margin: 20px 0 30px;
}

/* 按钮样式 */
.main-btn {
  width: 100%;
  padding: 14px 0;
  font-size: 16px;
  background: #ff7eb8 !important;
  border: none;
  box-shadow: 0 4px 12px rgba(255, 126, 184, 0.3);
  transition: all 0.3s;
}

.main-btn:hover {
  background: #ff66a8 !important;
  transform: translateY(-2px);
}

/* 底部装饰 */
.footer-decor {
  margin-top: 150px;
  display: flex;
  gap: 16px;
  font-size: 20px;
  animation: float 3s ease-in-out infinite;
}

@keyframes float {
  0%,
  100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-10px);
  }
}

/* 适配小屏幕 */
@media (max-width: 375px) {
  .title {
    font-size: 24px;
  }

  .card {
    padding: 28px 16px;
    margin-top: 120px;
  }
}
</style>