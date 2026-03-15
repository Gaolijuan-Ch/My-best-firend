<template>
  <div class="message-page">
    <!-- 顶部导航 -->
    <div class="nav-header">
      <img 
        src="../assets/hellokitty3.svg" 
        alt="logo" 
        class="nav-icon"
        @click="goBack" 
        title="点我返回上一页"
      />
    </div>

    <div class="card animate-card">
      <h3 class="card-title">✨ 写给 21 岁的盼盼 ✨</h3>
      
      <div class="letter-content">
        <p 
          v-for="(text, index) in fullText" 
          :key="index"
          :class="{
            'greeting': index === 0,
            'signature': index === fullText.length - 1
          }"
        >
          {{ text }}
        </p>
      </div>

      <!-- 装饰元素 -->
      <div class="decor-sticker">🎈</div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { useRouter } from 'vue-router'

const router = useRouter()

const goBack = () => {
  router.push('/memory')
}
console.log('测试提交')
// 完整的信件内容，按段落存储
// \n 代表换行
const fullText = [
 "亲爱的盼盼：",
  "生日快乐呀！\n时间过得好快， 转眼你就二十一岁啦。\n一到这种时候我就不知道该说什么了，\n很幸运呀在大学遇到了你。\n想给你一个印象深刻的生日礼物，\n这是我第一次为朋友做app哟嘿嘿，\n自己技术有限，想不出很好的创意。",
  "首先，夸夸你：\n一位超级善解人意，真诚，有自己的想法，大大方方的女孩，坚韧心中有自己目标。",
  "未来的生活，无论考研还是以后工作，\n都希望你活的热烈，\n在自己动摇的瞬间，都不要忘记自己的本心。\n当然也希望你开开心心，\n自己的开心呢才是最大的快乐，\n生命的价值在于你活的快乐，\n不快乐也可以来找我呀（我会超级开心你找我分享）！",
  "\n新的一岁，\n愿所愿皆所得 ✨"
]
</script>

<style scoped>
/* 页面背景 */
.message-page {
  min-height: 100vh;
  padding: 20px;
  background-image: url(../assets/b3.jpg);
  background-size: cover;
  background-position: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  position: relative;
}

/* 顶部导航 */
.nav-header {
  width: 100%;
  display: flex;
  justify-content: flex-start;
  margin-bottom: 10px;
}

.nav-icon {
  width: 60px;
  height: 60px;
  margin: 4px;
  transition: transform 0.3s;
  cursor: pointer;
  filter: drop-shadow(0 2px 4px rgba(0,0,0,0.1));
}

.nav-icon:active {
  transform: scale(0.85) rotate(-10deg);
}

/* 卡片主体 */
.card {
  width: 100%;
  max-width: 380px;
  min-height: 500px;
  background: rgba(255, 255, 255, 0.85); /* 半透明白底 */
  backdrop-filter: blur(5px);
  border-radius: 24px;
  padding: 30px 24px;
  box-shadow: 0 10px 40px rgba(227, 184, 197, 0.4);
  position: relative;
  margin-top: 10px;
  border: 2px solid rgba(255, 255, 255, 0.6);
}

/* 进场动画 */
.animate-card {
  animation: slideUp 0.8s cubic-bezier(0.2, 0.8, 0.2, 1);
}

@keyframes slideUp {
  from { opacity: 0; transform: translateY(40px); }
  to { opacity: 1; transform: translateY(0); }
}

.card-title {
  font-size: 20px;
  color: #ff7eb8;
  margin-top: 0;
  margin-bottom: 25px;
  text-align: center;
  font-weight: 700;
  letter-spacing: 1px;
  text-shadow: 1px 1px 0px #fff;
}

/* 文字内容样式 */
.letter-content {
  font-size: 15px;
  color: #5d4037; /* 温暖的深棕色文字 */
  line-height: 1.8; /* 宽松的行高，易于阅读 */
  text-align: justify; /* 两端对齐 */
  font-family: 'Comic Sans MS', '幼圆', sans-serif; /* 尝试使用圆润字体 */
}

/* 关键样式：保留换行符和空格 */
.letter-content p {
  margin-bottom: 16px;
  white-space: pre-wrap; 
  min-height: 1.8em; /* 避免没字的时候高度塌陷 */
}

/* 称呼样式（替代原来的 first-child） */
.greeting {
  font-weight: bold;
  font-size: 16px;
}

/* 签名样式 */
.signature {
  text-align: right;
  margin-top: 30px;
  font-weight: bold;
  color: #ff7eb8;
  font-style: italic;
}

/* 装饰贴纸 */
.decor-sticker {
  position: absolute;
  bottom: 15px;
  right: 15px;
  font-size: 40px;
  animation: float 3s ease-in-out infinite;
  opacity: 0.8;
}

@keyframes float {
  0%, 100% { transform: translateY(0) rotate(0deg); }
  50% { transform: translateY(-8px) rotate(5deg); }
}
</style>