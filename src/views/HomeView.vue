<template>
  <div class="birthday-container">
    
    <!-- ✨ 弹幕层 -->
    <!-- v-show="showDanmaku" 控制显示隐藏 -->
    <!-- pointer-events: none 保证弹幕不会挡住下面的点击操作 -->
    <div class="danmaku-container" v-show="showDanmaku">
      <div 
        v-for="(item, index) in danmakuList" 
        :key="index" 
        class="danmaku-item"
        :style="item.style"
      >
        {{ item.text }}
      </div>
    </div>

    <!-- 主卡片内容 -->
    <div class="card">
      <h1 class="title">
        🎂生日快乐🎂
        <br />
        &nbsp; 盼盼小宝宝！
      </h1>

      <p class="subtitle">
        今天要开心到飞起来！
        <br />
        愿我们的友谊永远亮闪闪 ✨
      </p>

      <!-- 图片展示区 -->
      <!-- 这里加了点击事件 @click="toggleDanmaku" -->
      <div class="image-container" @click="toggleDanmaku">
        <van-image
          round
          src="https://s3.bmp.ovh/imgs/2025/11/17/cdc65edaa0aa4dc5.png"
          width="180px"
          height="180px"
          alt="生日蛋糕"
          fit="cover"
        />
        <!-- 提示文字 -->
        
      </div>

      <!-- 按钮区域 -->
      <van-button type="primary" round class="main-btn" @click="goMemory">
        💗 打开二十一岁的第一封信
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
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

// 跳转到回忆页面
const goMemory = () => {
  router.push('/memory')
}

// --- 弹幕逻辑 ---
interface DanmakuItem {
  text: string;
  style: any;
}

const danmakuList = ref<DanmakuItem[]>([])
const showDanmaku = ref(false) // 控制弹幕显示的开关

// 切换弹幕显示状态
const toggleDanmaku = () => {
  showDanmaku.value = !showDanmaku.value
}

// 弹幕文案库，你可以随意添加
const texts = [
  "生日快乐 🎂", "天天开心 ✨", "暴富暴瘦 💰", "学业有成 📚", 
  "永远十八岁 🌸", "可可爱爱 🧸", "好运连连 🍀", "心想事成 🌟",
  "越来越美 💃", "平安喜乐 🍎", "吃不胖 🍰", "前程似锦 🌈", 
  "百事无忌 🧧", "万事胜意 🍊"
]

onMounted(() => {
  // 生成 25 条随机弹幕
  for (let i = 0; i < 25; i++) {
    const text = texts[Math.floor(Math.random() * texts.length)]
    
    danmakuList.value.push({
      text: text,
      style: {
        // 随机高度：只出现在屏幕上方 5% 到 45% 的位置
        top: `${5 + Math.random() * 40}%`, 
        // 随机动画时长：6秒 到 14秒 之间，造成速度差异
        animationDuration: `${6 + Math.random() * 8}s`,
        // 随机延迟：0秒 到 10秒 之间开始，避免同时出现
        animationDelay: `${Math.random() * 10}s`,
        // 随机字体大小
        fontSize: `${14 + Math.random() * 22}px`,
        // 随机颜色：白色或淡粉色
        color: Math.random() > 0.6 ? '#fff' : '#ff7db4',
        // 随机透明度
        opacity: 0.7 + Math.random() * 0.3
      }
    })
  }
})
</script>

<style scoped>
/* 页面基础样式 */
.birthday-container {
  min-height: 100vh;
  padding: 20px;
  background-image: url(../assets/壁纸4.jpg);
  background-size: cover;
  background-position: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  position: relative;
  overflow: hidden; /* 防止弹幕飞出屏幕出现滚动条 */
}

/* --- 弹幕样式 --- */
.danmaku-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 50%; /* 占据上部空间 */
  pointer-events: none; /* 关键：让鼠标点击能穿透弹幕层，不影响下面按钮 */
  z-index: 10;
  overflow: hidden;
}

.danmaku-item {
  position: absolute;
  white-space: nowrap; /* 不换行 */
  left: 100%; /* 初始位置在屏幕右侧外面 */
  font-weight: bold;
  text-shadow: 1px 1px 2px rgba(0,0,0,0.3); /* 文字阴影让弹幕更清晰 */
  animation: moveLeft linear infinite; /* 无限循环滚动 */
}

@keyframes moveLeft {
  from {
    transform: translateX(0);
  }
  to {
    /* 移动到屏幕左侧很远的地方，确保完全移出 */
    transform: translateX(-150vw); 
  }
}

/* 主卡片样式 */
.card {
  width: 90%;
  max-width: 400px;
  transform: translateY(100px);
  background: rgba(255, 255, 255, 0.4); 
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border-radius: 24px;
  padding: 20px 20px;
  box-shadow: 0 8px 32px rgba(255, 100, 150, 0.15);
  text-align: center;
  margin-top: 100px; /* 稍微调整一下位置 */
  position: relative;
  border: 1px solid rgba(255, 255, 255, 0.2);
  z-index: 5; /* 确保卡片在背景之上 */
}

/* 文本样式 */
.title {
  color: #ff5b9b;
  font-size: 28px;
  font-weight: 700;
  margin: 0 0 16px;
  line-height: 1.6;
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
  cursor: pointer; /* 鼠标变为手型 */
  transition: transform 0.2s; /* 点击时的缩放动画 */
}

/* 点击时的按压效果 */
.image-container:active {
  transform: scale(0.95);
}

.hint-text {
  font-size: 12px;
  color: #ff7db4;
  margin-top: 10px;
  opacity: 0.8;
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
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-10px); }
}

/* 适配小屏幕 */
@media (max-width: 375px) {
  .title { font-size: 24px; }
  .card { padding: 28px 16px; margin-top: 80px; }
}
</style>