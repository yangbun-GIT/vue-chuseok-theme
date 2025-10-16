<template>
  <div id="app-container">
    <div class="chuseok-scene">
      <!-- 배경 요소들 -->
      <div class="stars">
        <div v-for="n in 100" :key="`star-${n}`" class="star" :style="starStyles[n-1]"></div>
      </div>
      <div class="fireflies">
        <div v-for="n in 25" :key="`firefly-${n}`" class="firefly" :style="fireflyStyles[n-1]"></div>
      </div>
      <div class="mountain-silhouette"></div>

      <!-- 동적으로 스케일링될 콘텐츠 래퍼 -->
      <div class="content-wrapper" :style="contentStyle">
        <!-- 달 (클릭 이벤트 추가) -->
        <div class="moon" @click="makeAWish" title="클릭해서 소원 빌기"></div>

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
      </div>

      <!-- 위치 고정 UI 요소들 -->
      <div class="wish-prompt" :class="{ visible: !hasWished }">
        달을 클릭하여 소원을 빌어보세요
      </div>
      <div class="shooting-star" :class="{ animate: isWishing }"></div>
      <div class="lantern-container">
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
import { ref, onMounted, onUnmounted, nextTick, type CSSProperties } from 'vue';

// --- 상태 관리 ---
const fireflyStyles = ref<CSSProperties[]>([]);
const starStyles = ref<CSSProperties[]>([]);
const contentStyle = ref<CSSProperties>({});

const isWishing = ref(false);
const hasWished = ref(false);
const showLanternButton = ref(false);
const isWritingWish = ref(false);
const wishText = ref('');
const lanterns = ref<{id: number; text: string; style: CSSProperties}[]>([]);
const wishInputRef = ref<HTMLInputElement | null>(null);

// --- 화면 리사이즈 및 스케일링 핸들러 ---
const handleResize = () => {
  const vw = window.innerWidth;
  const vh = window.innerHeight;

  // 기준이 될 디자인 해상도 (가로가 넓은 PC 기준)
  const designWidth = 1920;
  const designHeight = 1080;

  // 현재 화면 비율과 디자인 비율 계산
  const scaleX = vw / designWidth;
  const scaleY = vh / designHeight;

  // 화면을 가득 채우면서도 비율을 유지하는 스케일 값 (cover)
  const scale = Math.max(scaleX, scaleY);

  // 세로가 긴 모바일 화면 등에서 콘텐츠가 너무 위로 쏠리지 않도록 Y축 위치 보정
  // 화면이 디자인 비율보다 세로로 길 경우, 콘텐츠를 좀 더 아래로 내림
  const yOffset = vh > vw * (designHeight / designWidth) ? (vh - vw * (designHeight / designWidth)) / 2 : 0;

  contentStyle.value = {
    transform: `translate(-50%, -50%) scale(${scale})`,
    top: `calc(50% + ${yOffset * 0.4}px)`, // 보정값을 좀 더 부드럽게 적용
  };
};

// --- 이벤트 핸들러 ---
const makeAWish = () => {
  if (isWishing.value) return;
  isWishing.value = true;
  hasWished.value = true;
  setTimeout(() => {
    isWishing.value = false;
    showLanternButton.value = true;
  }, 3000);
};

const showWishInput = async () => {
  isWritingWish.value = true;
  await nextTick();
  wishInputRef.value?.focus();
};

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

// --- 라이프사이클 훅 ---
onMounted(() => {
  // 초기 리사이즈 실행
  handleResize();
  // 리사이즈 이벤트 리스너 추가
  window.addEventListener('resize', handleResize);

  // 별 생성 (애니메이션 최적화를 위해 transform 사용)
  const newStarStyles: CSSProperties[] = [];
  for (let i = 0; i < 100; i++) {
    newStarStyles.push({
      transform: `translate(${Math.random() * 100}vw, ${Math.random() * 80}vh) scale(${Math.random() * 0.5 + 0.5})`,
      animation: `twinkle ${Math.random() * 5 + 2}s infinite alternate`,
      animationDelay: `${Math.random() * 5}s`,
    });
  }
  starStyles.value = newStarStyles;

  // 반딧불 생성
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
});

onUnmounted(() => {
  // 컴포넌트 언마운트 시 리스너 제거
  window.removeEventListener('resize', handleResize);
});
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Noto+Serif+KR:wght@400;600&display=swap');

:root {
  --night-sky-start: #000000;
  --night-sky-end: #0a0a10;
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
}

.chuseok-scene {
  position: relative;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  background: radial-gradient(ellipse at 50% 70%, var(--night-sky-end), var(--night-sky-start));
}

.stars, .fireflies {
  position: absolute; top: 0; left: 0;
  width: 100%; height: 100%;
  pointer-events: none;
}

.star {
  position: absolute;
  width: 2px; height: 2px;
  background: white;
  border-radius: 50%;
  box-shadow: 0 0 6px 2px white;
}

.firefly {
  position: absolute;
  width: 4px; height: 4px;
  background-color: #fefcd7;
  border-radius: 50%;
  box-shadow: 0 0 8px 3px #fefcd7;
  opacity: 0;
  animation: drift 15s ease-in-out infinite;
}

.content-wrapper {
  position: absolute;
  left: 50%;
  /* JS에서 top과 transform을 제어 */
  width: 1000px; /* 고정된 디자인 너비 */
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.moon {
  width: 250px; height: 250px;
  border-radius: 50%;
  cursor: pointer;
  background: radial-gradient(circle at 30% 30%, #fffde7, #f7f3d7 20%, #e0dcc3 70%, #d1cfb8 100%);
  box-shadow: 0 0 40px 10px #fefcd7, 0 0 80px 20px rgba(255, 253, 231, 0.5), inset -10px 10px 20px rgba(0,0,0,0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}
.moon:hover {
  transform: scale(1.05);
}

.text-content {
  color: var(--text-color);
  text-align: center;
  margin-top: 40px;
  text-shadow: 0 2px 10px rgba(0,0,0,0.5);
}
.text-content h1 { font-size: 3rem; font-weight: 600; margin: 0; }
.text-content p { font-size: 1.5rem; font-weight: 400; margin-top: 10px; }

.lantern-button {
  margin-top: 20px; padding: 10px 20px;
  background-color: rgba(255, 172, 51, 0.8); border: none;
  border-radius: 20px; color: white; font-family: 'Noto Serif KR', serif;
  font-size: 1.2rem; cursor: pointer; transition: all 0.3s ease;
  box-shadow: 0 2px 10px rgba(0,0,0,0.3);
}
.lantern-button:hover { transform: translateY(-2px); }

.wish-input-container {
  margin-top: 20px; display: flex; gap: 10px; align-items: center;
}
.wish-input-container input {
  background: transparent; border: none;
  border-bottom: 1px solid rgba(255, 255, 255, 0.5);
  color: white; font-family: 'Noto Serif KR', serif; font-size: 1.2rem;
  padding: 8px; width: 250px; text-align: center; transition: border-color 0.3s;
}
.wish-input-container input:focus { outline: none; border-bottom-color: var(--lantern-color); }
.wish-input-container button {
  background: var(--lantern-color); color: #333; border: none;
  border-radius: 15px; padding: 8px 15px; cursor: pointer;
  font-family: 'Noto Serif KR', serif; font-size: 1.2rem;
}

.wish-prompt {
  position: absolute; bottom: 3vh; left: 50%; transform: translateX(-50%);
  color: rgba(255, 255, 255, 0.7); font-size: 1rem;
  opacity: 0; transition: opacity 1s ease 3s; pointer-events: none;
}
.wish-prompt.visible { opacity: 1; }

.mountain-silhouette {
  position: absolute; bottom: 0; left: 0;
  width: 100%; height: 25vh;
  background: linear-gradient(to top, #020111 20%, transparent), url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1200 120"><path fill="%23051a24" d="M0,120 L1200,120 L1200,60 C1000,0 800,80 600,60 C400,40 200,120 0,80 Z"></path></svg>');
  background-size: cover; background-position: bottom center;
  opacity: 0.8;
}

.shooting-star {
  position: absolute; top: 10%; right: -200px;
  width: 3px; height: 3px; background: white;
  border-radius: 50%; box-shadow: 0 0 10px 2px white;
  opacity: 0; z-index: 5;
}
.shooting-star.animate { animation: shoot 3s ease-in-out forwards; }
.shooting-star::after {
  content: ''; position: absolute; top: 50%; right: 1px;
  transform: translateY(-50%); width: 200px; height: 2px;
  background: linear-gradient(to right, white, transparent);
}

.lantern-container {
  position: absolute; bottom: 0; left: 0; width: 100%; height: 100%;
  pointer-events: none; z-index: 4;
}
.lantern {
  position: absolute; bottom: -120px; width: 60px;
  opacity: 0; animation: rise-and-fade linear forwards;
  filter: drop-shadow(0 0 15px var(--lantern-color));
}
.lantern svg { width: 100%; height: auto; }


/* --- Animations --- */
@keyframes twinkle {
  from { opacity: 0.2; }
  to { opacity: 0.8; }
}

@keyframes drift {
  0% { transform: translate(0, 0); opacity: 0; }
  25% { opacity: 0.9; }
  50% { transform: translate(40px, -40px); }
  75% { opacity: 0.9; }
  100% { transform: translate(0, -80px); opacity: 0; }
}

@keyframes shoot {
  0% { opacity: 1; transform: translate(0, 0) scale(1); }
  100% { opacity: 0; transform: translate(-120vw, 80vh) scale(0.5); }
}

@keyframes rise-and-fade {
  0% { transform: translateY(0) scale(1); opacity: 0; }
  10% { opacity: 1; }
  100% { transform: translateY(-90vh) scale(0.2); opacity: 0; }
}
</style>

