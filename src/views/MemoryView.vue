<template>
  <div class="memory-page">
    <audio ref="audioRef" :src="bgmUrl" loop></audio>
    <div class="music-btn" @click="toggleMusic" :class="{ 'playing': isMusicPlaying }">
      <span>🎵</span>
    </div>

    <div class="title-bar">
      <img src="../assets/hellokitty.svg" alt="logo" class="nav-icon" @click="goBack" title="返回上一页"/>
    </div>

    <div class="card">
      <h3>✨ 照片照片照片✨</h3>
      
      <div class="photo-frame">
        <div v-if="loading" class="loading-text">正在打开时光胶囊...</div>
        
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

      <div class="controls">
        <button class="play-btn" @click="togglePlay">
          {{ isSlidePlaying ? '暂停啦' : '▶开始播放' }}
        </button>

        <div class="slider-container">
          <span class="time-label">Start</span>
          <input 
            type="range" 
            min="0" 
            :max="totalImages - 1" 
            v-model.number="currentIndex" 
            class="timeline-slider"
            @input="pauseSlide"
          >
          <span class="time-label">Now</span>
        </div>
      </div>

      <p class="subtitle">可以拖动我哦</p>
    </div>

    <img src="../assets/next.svg" alt="next" class="next-btn" @click="goto" />
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue'
import { useRouter } from 'vue-router'
import bgmUrl from '../assets/songs.mp3'

const router = useRouter()
const audioRef = ref<HTMLAudioElement | null>(null)
const isMusicPlaying = ref(false)

const toggleMusic = () => {
  if (!audioRef.value) return
  if (isMusicPlaying.value) {
    audioRef.value.pause()
    isMusicPlaying.value = false
  } else {
    audioRef.value.volume = 0.5 
    audioRef.value.play().then(() => {
      isMusicPlaying.value = true
    }).catch(err => {
      console.log('播放失败，可能需要用户交互:', err)
    })
  }
}

// --- 幻灯片逻辑 ---
const currentIndex = ref(0)
const isSlidePlaying = ref(false) 
const loading = ref(true)
let timer: number | null = null 

// *** 修改部分开始：直接生成 MinIO URL 列表 ***
// 这里的逻辑已改为从 p1.jpg 生成到 p24.jpg
const images: string[] = []
const baseUrl = 'http://43.138.85.114:9000/mipanpan/'

for (let i = 1; i <= 24; i++) {
  images.push(`${baseUrl}p${i}.jpg`)
}
// *** 修改部分结束 ***

const totalImages = computed(() => images.length)
const currentImage = computed(() => images[currentIndex.value])

// 初始化
onMounted(() => {
  // 因为是 URL 字符串列表，不需要等待 import，直接设为加载完成
  loading.value = false
  
  // 🎵 尝试自动播放音乐
  if (audioRef.value) {
    audioRef.value.volume = 0.5
    // 浏览器的策略：通常需要用户产生交互（点击过页面）才能自动播放有声媒体
    // 我们尝试直接播放
    audioRef.value.play().then(() => {
      isMusicPlaying.value = true
    }).catch(() => {
      console.log('自动播放被拦截，等待用户点击')
      document.addEventListener('click', () => {
        if (!isMusicPlaying.value && audioRef.value) {
          audioRef.value.play()
          isMusicPlaying.value = true
        }
      }, { once: true })
    })
  }
})

onUnmounted(() => {
  pauseSlide()
  if (audioRef.value) {
    audioRef.value.pause()
  }
})


const togglePlay = () => {
  if (isSlidePlaying.value) {
    pauseSlide()
  } else {
    startSlide()
  }
}

const startSlide = () => {
  if (timer) return
  isSlidePlaying.value = true
  timer = setInterval(() => {
    if (currentIndex.value < totalImages.value - 1) {
      currentIndex.value++
    } else {
      currentIndex.value = 0 
    }
  }, 1200)
}

const pauseSlide = () => {
  isSlidePlaying.value = false
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
  background-image: url(../assets/cake.jpg);
  background-size: cover;
  background-position: center;
  position: relative;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
}

/* 🎵 音乐按钮样式 */
.music-btn {
  position: absolute;
  top: 20px;
  right: 20px;
  width: 40px;
  height: 40px;
  background: rgba(255, 255, 255, 0.3);
  backdrop-filter: blur(5px);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  z-index: 100;
  border: 1px solid rgba(255,255,255,0.5);
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
  font-size: 20px;
}

/* 旋转动画：只有当 .playing 类存在时才旋转 */
.playing {
  animation: rotate 3s linear infinite;
  background: rgba(255, 222, 235, 0.6); /* 播放时变粉一点 */
  border-color: #ff7eb8;
}

@keyframes rotate {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
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

/* 卡片样式优化 */
.card {
  flex: 1;
  max-height: 700px;
  background: rgba(255, 255, 255, 0.25);
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
  margin-bottom: 60px;
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
  aspect-ratio: 3/4;
  background: #fff;
  padding: 10px 10px 35px 10px;
  border-radius: 4px;
  box-shadow: 0 10px 20px rgba(0,0,0,0.15);
  transform: rotate(-2deg);
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
  object-fit: cover;
  display: block;
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