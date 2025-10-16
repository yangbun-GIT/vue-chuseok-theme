<template>
  <div id="app-container">
    <div class="chuseok-scene">
      <!-- 반짝이는 별들 -->
      <div class="stars">
        <div v-for="n in 100" :key="`star-${n}`" class="star" :style="starStyles[n-1]"></div>
      </div>

      <!-- 달 (클릭 이벤트 추가) -->
      <div class="moon" @click="makeAWish" title="클릭해서 소원 빌기"></div>

      <!-- 소원 빌기 안내 문구 -->
      <div class="wish-prompt" :class="{ visible: !hasWished }">
        달을 클릭하여 소원을 빌어보세요
      </div>

      <div class="text-content">
        <h1>풍요로운 한가위</h1>
        <p>보름달처럼 넉넉하고 행복한 명절 보내세요.</p>
      </div>

      <!-- 소원 등불 버튼 -->
      <button v-if="showLanternButton && !isWritingWish" @click="showWishInput" class="lantern-button">소원 등불 날리기</button>

      <!-- 새로운 소원 입력 UI -->
      <div v-if="isWritingWish" class="wish-input-container">
        <input
            type="text"
            v-model="wishText"
            placeholder="소원을 여기에 적어보세요..."
            @keyup.enter="sendWish"
            ref="wishInputRef"
        />
        <button @click="sendWish">날리기</button>
      </div>

      <!-- 반딧불 효과를 위한 컨테이너 (개수 25개로 수정) -->
      <div class="fireflies">
        <div v-for="n in 25" :key="`firefly-${n}`" class="firefly" :style="fireflyStyles[n-1]"></div>
      </div>

      <!-- 산 실루엣 -->
      <div class="mountain-silhouette"></div>

      <!-- 별똥별 -->
      <div class="shooting-star" :class="{ animate: isWishing }"></div>

      <!-- 연등 컨테이너 -->
      <div class="lantern-container">
        <!-- 새로운 SVG 연등 디자인 -->
        <div v-for="lantern in lanterns" :key="lantern.id" class="lantern" :style="lantern.style" :title="lantern.text">
          <svg viewBox="0 0 80 120" xmlns="http://www.w3.org/2000/svg">
            <defs>
              <radialGradient id="lanternGlow" cx="50%" cy="50%" r="50%" fx="50%" fy="50%">
                <stop offset="0%" style="stop-color:rgba(255,230,180,0.9)" />
                <stop offset="100%" style="stop-color:rgba(255,172,51,0.8)" />
              </radialGradient>
            </defs>
            <path d="M10 30 Q 5 50 10 70 L 70 70 Q 75 50 70 30 Z" fill="url(#lanternGlow)"></path>
            <rect x="5" y="25" width="70" height="10" rx="3" fill="#6b4120"></rect>
            <rect x="5" y="70" width="70" height="5" rx="2" fill="#6b4120"></rect>
            <path d="M30 80 L 50 80 L 55 110 L 25 110 Z" fill="#d4af37" opacity="0.7"></path>
          </svg>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
// 'CSSProperties' 앞에 'type'을 추가하여 타입 전용 import로 명시
import { ref, onMounted, nextTick, type CSSProperties } from 'vue';

// 스타일
const fireflyStyles = ref<CSSProperties[]>([]);
const starStyles = ref<CSSProperties[]>([]);

// 상태
const isWishing = ref(false);
const hasWished = ref(false);
const showLanternButton = ref(false);
const isWritingWish = ref(false);
const wishText = ref('');
const lanterns = ref<{id: number; text: string; style: CSSProperties}[]>([]);
const wishInputRef = ref<HTMLInputElement | null>(null);

// 별똥별 소원 빌기
const makeAWish = () => {
  if (isWishing.value) return;

  isWishing.value = true;
  hasWished.value = true;

  setTimeout(() => {
    isWishing.value = false;
    showLanternButton.value = true;
  }, 3000);
};

// 소원 입력창 보이기
const showWishInput = async () => {
  isWritingWish.value = true;
  await nextTick();
  wishInputRef.value?.focus();
};

// 연등 날리기
const sendWish = () => {
  if (!wishText.value.trim()) return;

  const newLantern = {
    id: Date.now(),
    text: wishText.value,
    style: {
      left: `${Math.random() * 80 + 10}%`,
      animationDuration: `${Math.random() * 5 + 10}s`,
    }
  };
  lanterns.value.push(newLantern);

  setTimeout(() => {
    lanterns.value = lanterns.value.filter(l => l.id !== newLantern.id);
  }, 15000);

  wishText.value = '';
  isWritingWish.value = false;
};

onMounted(() => {
  // 반딧불 생성 (25개로 수정)
  const newFireflyStyles: CSSProperties[] = [];
  for (let i = 0; i < 25; i++) {
    newFireflyStyles.push({
      top: `${Math.random() * 100}%`,
      left: `${Math.random() * 100}%`,
      animationDuration: `${Math.random() * 10 + 5}s`,
      animationDelay: `${Math.random() * 15}s`,
    });
  }
  fireflyStyles.value = newFireflyStyles;

  // 별 생성
  const newStarStyles: CSSProperties[] = [];
  for (let i = 0; i < 100; i++) {
    newStarStyles.push({
      top: `${Math.random() * 80}%`,
      left: `${Math.random() * 100}%`,
      animation: `twinkle ${Math.random() * 5 + 2}s infinite alternate`,
      animationDelay: `${Math.random() * 5}s`,
      transform: `scale(${Math.random() * 0.5 + 0.5})`,
    });
  }
  starStyles.value = newStarStyles;
});
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Noto+Serif+KR:wght@400;600&display=swap');

:root {
  --night-sky-start: #000000;
  --night-sky-end: #0a0a10;
  --mountain-color: #051a24;
  --moon-color: #fffde7;
  --moon-glow: #fefcd7;
  --text-color: #ffffff;
  --lantern-color: #ffac33;
}

body, html {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  background-color: var(--night-sky-start);
}

#app-container {
  font-family: 'Noto Serif KR', serif;
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: radial-gradient(ellipse at 50% 70%, var(--night-sky-end), var(--night-sky-start));
}

.chuseok-scene {
  position: relative;
  /* 화면을 가득 채우도록 수정 */
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  overflow: hidden;
  box-shadow: inset 0 0 250px #000;
}

.stars, .fireflies {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 0;
}
.star {
  position: absolute;
  width: 0.2vmin;
  height: 0.2vmin;
  background: white;
  border-radius: 50%;
  box-shadow: 0 0 0.6vmin 0.2vmin white;
}
@keyframes twinkle {
  from { opacity: 0.2; }
  to { opacity: 0.8; }
}
.firefly {
  position: absolute;
  width: 0.4vmin;
  height: 0.4vmin;
  background-color: var(--moon-glow);
  border-radius: 50%;
  box-shadow: 0 0 0.8vmin 0.3vmin var(--moon-glow);
  opacity: 0;
  animation: drift ease-in-out infinite;
}
@keyframes drift {
  0% { transform: translate(0, 0); opacity: 0; }
  25% { opacity: 0.9; }
  50% { transform: translate(4vmin, -4vmin); }
  75% { opacity: 0.9; }
  100% { transform: translate(0, -8vmin); opacity: 0; }
}

.moon {
  /* vmin을 사용하여 동적 크기 조절 */
  width: 20vmin;
  height: 20vmin;
  min-width: 150px;
  min-height: 150px;
  border-radius: 50%;
  position: relative;
  z-index: 2;
  cursor: pointer;
  background: radial-gradient(circle at 30% 30%, #fffde7, #f7f3d7 20%, #e0dcc3 70%, #d1cfb8 100%);
  box-shadow: 0 0 4vmin 1vmin var(--moon-glow),
  0 0 8vmin 2vmin rgba(255, 253, 231, 0.5),
  inset -1vmin 1vmin 2vmin rgba(0,0,0,0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}
.moon:hover {
  transform: scale(1.05);
  box-shadow: 0 0 5vmin 1.5vmin var(--moon-glow),
  0 0 12vmin 3.5vmin rgba(255, 253, 231, 0.6),
  inset -1vmin 1vmin 2vmin rgba(0,0,0,0.1);
}

.text-content {
  color: var(--text-color);
  text-align: center;
  margin-top: 4vmin;
  position: relative;
  z-index: 3;
  text-shadow: 0 2px 10px rgba(0,0,0,0.5);
  animation: fade-in 3s ease-out forwards;
  opacity: 0;
}
@keyframes fade-in { to { opacity: 1; } }
.text-content h1 {
  /* clamp를 사용하여 폰트 크기 반응형으로 조절 */
  font-size: clamp(2rem, 4vmin, 3.5rem);
  font-weight: 600; margin: 0;
}
.text-content p {
  font-size: clamp(1rem, 2vmin, 1.5rem);
  font-weight: 400; margin-top: 1vmin;
}

.wish-prompt {
  position: absolute;
  bottom: 2vmin;
  color: rgba(255, 255, 255, 0.7);
  font-size: clamp(0.8rem, 1.5vmin, 1rem);
  z-index: 10;
  opacity: 0;
  transition: opacity 1s ease 3s;
  pointer-events: none;
}
.wish-prompt.visible { opacity: 1; }

.mountain-silhouette {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 25%;
  background: linear-gradient(to top, #020111, transparent), url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1200 120"><path fill="%23051a24" d="M0,120 L1200,120 L1200,60 C1000,0 800,80 600,60 C400,40 200,120 0,80 Z"></path></svg>');
  background-size: 100% 100%;
  background-repeat: no-repeat;
  z-index: 0;
  opacity: 0.8;
}

.shooting-star {
  position: absolute; top: 10%; right: -20vmin; width: 0.3vmin; height: 0.3vmin; background: white;
  border-radius: 50%; box-shadow: 0 0 1vmin 0.2vmin white; opacity: 0; z-index: 5;
}
.shooting-star.animate { animation: shoot 3s ease-in-out forwards; }
.shooting-star::after {
  content: ''; position: absolute; top: 50%; right: 0.1vmin; transform: translateY(-50%);
  width: 20vmin; height: 0.2vmin; background: linear-gradient(to right, white, transparent);
}
@keyframes shoot {
  0% { opacity: 1; transform: translate(0, 0) scale(1); }
  100% { opacity: 0; transform: translate(-120vw, 80vh) scale(0.5); }
}

.lantern-button {
  margin-top: 2vmin;
  padding: 1vmin 2vmin;
  background-color: rgba(255, 172, 51, 0.8);
  border: none;
  border-radius: 2vmin;
  color: white;
  font-family: 'Noto Serif KR', serif;
  font-size: clamp(0.9rem, 1.8vmin, 1.2rem);
  cursor: pointer;
  z-index: 10;
  transition: all 0.3s ease;
  box-shadow: 0 0.2vmin 1vmin rgba(0,0,0,0.3);
}
.lantern-button:hover {
  background-color: var(--lantern-color);
  transform: translateY(-0.2vmin);
  box-shadow: 0 0.4vmin 1.5vmin rgba(0,0,0,0.4);
}

.wish-input-container {
  margin-top: 2vmin;
  display: flex;
  gap: 1vmin;
  align-items: center;
  z-index: 10;
  animation: fade-in 0.5s ease-out;
}
.wish-input-container input {
  background: transparent;
  border: none;
  border-bottom: 1px solid rgba(255, 255, 255, 0.5);
  color: white;
  font-family: 'Noto Serif KR', serif;
  font-size: clamp(0.9rem, 1.8vmin, 1.2rem);
  padding: 0.8vmin;
  width: 25vmin;
  min-width: 180px;
  text-align: center;
  transition: border-color 0.3s;
}
.wish-input-container input:focus {
  outline: none;
  border-bottom-color: var(--lantern-color);
}
.wish-input-container button {
  background: var(--lantern-color);
  color: #333;
  border: none;
  border-radius: 1.5vmin;
  padding: 0.8vmin 1.5vmin;
  cursor: pointer;
  font-family: 'Noto Serif KR', serif;
  font-size: clamp(0.9rem, 1.8vmin, 1.2rem);
  transition: background-color 0.2s;
}
.wish-input-container button:hover {
  background-color: #ffc266;
}

.lantern-container {
  position: absolute; bottom: 0; left: 0; width: 100%; height: 100%;
  pointer-events: none; z-index: 4;
}
.lantern {
  position: absolute;
  bottom: -12vmin; /* SVG 크기에 맞춰 조정 */
  width: 6vmin; /* SVG 크기에 맞춰 조정 */
  min-width: 50px;
  opacity: 0;
  animation: rise-and-fade linear forwards;
  filter: drop-shadow(0 0 1.5vmin var(--lantern-color));
}
.lantern svg {
  width: 100%;
  height: auto;
}

@keyframes rise-and-fade {
  0% { transform: translateY(0) scale(1); opacity: 0; }
  10% { opacity: 1; }
  100% { transform: translateY(-90vmin) scale(0.2); opacity: 0; }
}
</style>


