<template>
  <div class="memory-page">
    <!-- 顶部导航 -->
    <div class="title-bar">
      <img src="../assets/hellokitty.svg" alt="logo" class="nav-icon" @click="goBack" title="返回上一页"/>
    
    </div>

    <div class="card">
      <h3>✨ 照片墙 ✨</h3>
      
      <!-- 照片展示区 -->
      <div class="photo-frame">
        <!-- 加载中提示 -->
        <div v-if="loading" class="loading-text">正在打开时光胶囊...</div>
        
        <!-- 图片显示 -->
        <div v-else class="image-container" @click="togglePlay">
          <transition name="fade" mode="out-in">
            <img 
              :key="currentIndex" 
              :src="currentImage" 
              class="memory-photo" 
              alt="Memory"
            />
          </transition>
        </div>

        
      </div>

      <!-- 控制区 -->
      <div class="controls">
        <!-- 播放/暂停按钮 -->
        <button class="play-btn" @click="togglePlay">
          {{ isPlaying ? '暂停啦' : '▶开始播放' }}
        </button>

        <!-- 进度条 -->
        <div class="slider-container">
          <span class="time-label">Start</span>
          <input 
            type="range" 
            min="0" 
            :max="totalImages - 1" 
            v-model.number="currentIndex" 
            class="timeline-slider"
            @input="pause"
          >
          <span class="time-label">Now</span>
        </div>
      </div>

      <p class="subtitle">可以拖动我哦</p>
    </div>

    <img src="../assets/下一页.svg" alt="next" class="next-btn" @click="goto" />
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()


const currentIndex = ref(0)
const isPlaying = ref(false)
const loading = ref(true)
let timer: number | null = null 

// 1. 动态导入图片 (关键步骤)
// 使用 Vite 的 glob 导入 assets 目录下所有以 "图片" 开头的文件 (jpg/png等)
// eager: true 表示直接获取模块，而不是异步加载
const imageModules = import.meta.glob('../assets/图片*.*', { eager: true })

// 2. 处理图片列表并排序
const images = Object.keys(imageModules)
  .map((path) => {
    // 获取模块 default 导出 (即图片路径)
    // @ts-ignore
    return imageModules[path].default
  })
  .sort((a, b) => {
    // 提取文件名中的数字进行自然排序 (例如: 图片2 应在 图片10 前面)
    // 假设路径类似于 /src/assets/图片1.jpg
    const getNum = (str: string) => {
      const match = str.match(/图片(\d+)/);
      return match ? parseInt(match[1]) : 0;
    };
    return getNum(a) - getNum(b);
  });

const totalImages = computed(() => images.length)
const currentImage = computed(() => images[currentIndex.value])

// 初始化
onMounted(() => {
  if (images.length > 0) {
    loading.value = false
    // 可选：进来自动播放，如果想自动播，把下面这行注释解开
    // startPlay() 
  }
})

onUnmounted(() => {
  pause()
})

// --- 控制功能 ---

const togglePlay = () => {
  if (isPlaying.value) {
    pause()
  } else {
    startPlay()
  }
}

const startPlay = () => {
  if (timer) return
  isPlaying.value = true
  // 每 1.2 秒切换一张
  timer = setInterval(() => {
    if (currentIndex.value < totalImages.value - 1) {
      currentIndex.value++
    } else {
      // 播放完回到第一张，并暂停（或者你想循环播放就注释掉 pause）
      currentIndex.value = 0 
      // pause() 
    }
  }, 1200)
}

const pause = () => {
  isPlaying.value = false
  if (timer) {
    clearInterval(timer)
    timer = null
  }
}

const goBack = () => router.push('/')
const goto = () => router.push('/others')
</script>

<style scoped>
/* 保持原有背景设置 */
.memory-page {
  min-height: 100vh;
  padding: 22px;
  background-image: url(../assets/蛋糕.jpg);
  background-size: cover;
  background-position: center;
  position: relative;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
}

/* 顶部导航优化 */
.title-bar {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.nav-icon {
  width: 50px; 
  height: 50px; 
  transition: transform 0.2s;
  cursor: pointer;
  filter: drop-shadow(0 2px 2px rgba(0,0,0,0.1));
}
.nav-icon:active {
  transform: scale(0.9);
}

.page-title {
  color: #fff;
  font-weight: bold;
  font-size: 18px;
  margin-left: 10px;
  text-shadow: 0 2px 4px rgba(0,0,0,0.3);
}

/* 卡片样式优化 */
.card {
  /* 高度自适应，但给个最小高度 */
  flex: 1;
  max-height: 700px;
  background: rgba(255, 255, 255, 0.25); /* 稍微调高一点透明度让图片更清晰 */
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px); 
  border-radius: 24px;
  padding: 24px 20px;
  box-shadow: 0 8px 32px rgba(255, 100, 150, 0.2);
  border: 1px solid rgba(255, 255, 255, 0.4);
  color: #fff;
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  margin-bottom: 60px; /* 留出底部按钮位置 */
}

h3 {
  margin-top: 0;
  margin-bottom: 15px;
  color: #fff;
  text-shadow: 0 1px 3px rgba(255, 91, 155, 0.5);
  font-size: 18px;
  letter-spacing: 1px;
}

/* 照片相框风格 */
.photo-frame {
  width: 100%;
  max-width: 320px;
  aspect-ratio: 3/4; /* 保持一个长宽比，如果是横图较多可以改成 4/3 */
  background: #fff;
  padding: 10px 10px 35px 10px; /* 底部留白像拍立得 */
  border-radius: 4px;
  box-shadow: 0 10px 20px rgba(0,0,0,0.15);
  transform: rotate(-2deg); /* 微微倾斜增加俏皮感 */
  transition: transform 0.3s;
  position: relative;
  margin: 10px 0;
}

.photo-frame:hover {
  transform: rotate(0deg) scale(1.02);
  z-index: 10;
}

.image-container {
  width: 100%;
  height: 100%;
  overflow: hidden;
  background: #f0f0f0;
  cursor: pointer;
  position: relative;
}

.memory-photo {
  width: 100%;
  height: 100%;
  object-fit: cover; /* 保证图片填满 */
  display: block;
}

.timestamp {
  position: absolute;
  bottom: 6px;
  left: 0;
  width: 100%;
  text-align: center;
  color: #666;
  font-family: 'Courier New', Courier, monospace; /* 打印机字体 */
  font-size: 12px;
  display: flex;
  justify-content: space-between;
  padding: 0 14px;
  box-sizing: border-box;
}

/* 控制区 */
.controls {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 12px;
  margin-top: 10px;
}

.play-btn {
  background: linear-gradient(45deg, #ff9a9e 0%, #fecfef 99%, #fecfef 100%);
  border: none;
  padding: 8px 24px;
  border-radius: 20px;
  color: white;
  font-weight: bold;
  font-size: 14px;
  box-shadow: 0 4px 10px rgba(255, 100, 150, 0.3);
  cursor: pointer;
  transition: all 0.3s;
}

.play-btn:active {
  transform: scale(0.95);
}

.slider-container {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.time-label {
  font-size: 10px;
  color: rgba(255,255,255,0.8);
}

.timeline-slider {
  width: 70%;
  height: 6px;
  -webkit-appearance: none;
  background: rgba(255, 255, 255, 0.5);
  border-radius: 5px;
  outline: none;
}

.timeline-slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 18px;
  height: 18px;
  border-radius: 50%;
  background: #ff5b9b;
  cursor: pointer;
  border: 2px solid #fff;
  box-shadow: 0 2px 5px rgba(0,0,0,0.2);
}

.subtitle {
  font-size: 12px;
  color: rgba(255, 255, 255, 0.9);
  margin-top: 5px;
  font-style: italic;
}

.next-btn {
  width: 50px;
  height: 50px;
  position: absolute;
  bottom: 25px;
  right: 25px;
  filter: drop-shadow(0 2px 4px rgba(0,0,0,0.2));
  cursor: pointer;
  transition: transform 0.2s;
  z-index: 20;
}
.next-btn:active {
  transform: scale(0.9);
}

.loading-text {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #999;
}

/* 切换动画 */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>