<template>
  <div class="birthday-container">
    
    <!-- 🔒 锁定层 -->
    <div v-if="!isUnlocked" class="lock-container">
      <div class="lock-card">
        <div class="lock-icon">🔒</div>
        <h3>请输入生日密码</h3>
        <p class="lock-hint">解锁查看给盼盼的惊喜</p>
        
        <input 
          type="tel" 
          v-model="inputPassword" 
          class="pwd-input" 
          placeholder="请输入4位密码" 
          maxlength="4"
        />
        
        <van-button 
          type="primary" 
          round 
          block 
          class="unlock-btn" 
          @click="checkPassword"
        >
          立即解锁
        </van-button>
        
        <p v-if="showError" class="error-msg">密码错误，你不是米盼盼！！！🐷</p>
      </div>
    </div>

    <!-- 🎉 解锁后的内容 -->
    <template v-else>
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

        <div class="image-container" @click="toggleDanmaku">
          <van-image
            round
            src="https://s3.bmp.ovh/imgs/2025/11/17/cdc65edaa0aa4dc5.png"
            width="180px"
            height="180px"
            alt="生日蛋糕"
            fit="cover"
          />
        </div>

        <van-button type="primary" round class="main-btn" @click="goMemory">
          💗 打开二十一岁的第一封信
        </van-button>
      </div>

      <div class="footer-decor">
        <span>✨</span>
        <span>💖</span>
        <span>🎉</span>
        <span>💝</span>
        <span>✨</span>
      </div>
    </template>

  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import type { CSSProperties } from 'vue'

const router = useRouter()
const STORAGE_KEY = 'panpan_birthday_unlocked_v1' // 定义存储的key

// 1. 修改初始化逻辑：先检查 localStorage 是否有记录
const isUnlocked = ref(localStorage.getItem(STORAGE_KEY) === 'true')

const inputPassword = ref('') 
const showError = ref(false) 

const checkPassword = () => {
  if (inputPassword.value === '1029') {
    // 2. 密码正确，保存状态到 localStorage
    isUnlocked.value = true
    localStorage.setItem(STORAGE_KEY, 'true')
    
    // 立即初始化弹幕
    initDanmaku() 
  } else {
    showError.value = true
    inputPassword.value = '' 
    
    setTimeout(() => {
      showError.value = false
    }, 2000)
  }
}

const goMemory = () => {
  router.push('/memory')
}

interface DanmakuItem {
  text: string;
  style: CSSProperties;
}

const danmakuList = ref<DanmakuItem[]>([])
const showDanmaku = ref(false) 

const toggleDanmaku = () => {
  showDanmaku.value = !showDanmaku.value
}

const texts = [
  "生日快乐 🎂", "天天开心 ✨", "暴富暴瘦 💰", "学业有成 📚", 
  "永远十八岁 🌸", "可可爱爱 🧸", "好运连连 🍀", "心想事成 🌟",
  "越来越美 💃", "平安喜乐 🍎", "吃不胖 🍰", "前程似锦 🌈", 
  "百事无忌 🧧", "万事胜意 🍊"
]

const initDanmaku = () => {
  danmakuList.value = [] 
  for (let i = 0; i < 25; i++) {
    const text = texts[Math.floor(Math.random() * (texts.length || 1))] || "默认祝福语"
    danmakuList.value.push({
      text: text,
      style: {
        top: `${5 + Math.random() * 40}%`, 
        animationDuration: `${6 + Math.random() * 8}s`,
        animationDelay: `${Math.random() * 10}s`,
        fontSize: `${14 + Math.random() * 22}px`,
        color: Math.random() > 0.6 ? '#fff' : '#ff7db4',
        opacity: 0.7 + Math.random() * 0.3
      }
    })
  }
}

onMounted(() => {
  // 3. 如果已经是解锁状态，直接加载弹幕
  // 如果是锁定状态，不需要加载，等用户点击解锁后再加载
  if (isUnlocked.value) {
    initDanmaku()
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
  overflow: hidden; 
}

.lock-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.3);
  backdrop-filter: blur(15px);
  z-index: 100;
  display: flex;
  justify-content: center;
  align-items: center;
}

.lock-card {
  width: 80%;
  max-width: 320px;
  background: rgba(255, 255, 255, 0.9);
  padding: 30px 20px;
  border-radius: 20px;
  text-align: center;
  box-shadow: 0 10px 30px rgba(0,0,0,0.2);
  animation: popIn 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.lock-icon {
  font-size: 40px;
  margin-bottom: 10px;
}

.lock-card h3 {
  color: #ff5b9b;
  margin: 0 0 5px;
}

.lock-hint {
  color: #999;
  font-size: 14px;
  margin-bottom: 20px;
}

.pwd-input {
  width: 80%;
  padding: 12px;
  border: 2px solid #ffdeeb;
  border-radius: 12px;
  font-size: 18px;
  text-align: center;
  outline: none;
  margin-bottom: 20px;
  color: #ff5b9b;
  letter-spacing: 4px;
  transition: border-color 0.3s;
}

.pwd-input:focus {
  border-color: #ff7eb8;
}

.unlock-btn {
  background: linear-gradient(to right, #ff7eb8, #ff5b9b) !important;
  border: none !important;
  font-weight: bold;
}

.error-msg {
  color: #ff4d4f;
  font-size: 12px;
  margin-top: 15px;
  animation: shake 0.5s;
}

/* 简单的弹窗进入动画 */
@keyframes popIn {
  from { transform: scale(0.8); opacity: 0; }
  to { transform: scale(1); opacity: 1; }
}

/* 错误时的抖动动画 */
@keyframes shake {
  0%, 100% { transform: translateX(0); }
  25% { transform: translateX(-5px); }
  75% { transform: translateX(5px); }
}


/* --- 弹幕样式 --- */
.danmaku-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 50%; 
  pointer-events: none; 
  z-index: 10;
  overflow: hidden;
}

.danmaku-item {
  position: absolute;
  white-space: nowrap; 
  left: 100%; 
  font-weight: bold;
  text-shadow: 1px 1px 2px rgba(0,0,0,0.3); 
  animation: moveLeft linear infinite; 
}

@keyframes moveLeft {
  from {
    transform: translateX(0);
  }
  to {
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
  margin-top: 100px; 
  position: relative;
  border: 1px solid rgba(255, 255, 255, 0.2);
  z-index: 5; 
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
  cursor: pointer; 
  transition: transform 0.2s; 
}

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