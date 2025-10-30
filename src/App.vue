<template>
  <div class="chuseok-app">
    <!-- Google Fonts Import -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous">
    <link href="https://fonts.googleapis.com/css2?family=Gowun+Dodum&display=swap" rel="stylesheet">

    <!-- 예술적 효과: 별똥별 -->
    <div class="shooting-stars">
      <div class="star"></div>
      <div class="star"></div>
      <div class="star"></div>
    </div>

    <header class="app-header">
      <h1>풍요로운 한가위</h1>
      <p>"더도 말고 덜도 말고 한가위만 같아라"</p>
    </header>

    <main class="app-main">
      <!-- CSS로 만든 보름달 -->
      <div class="full-moon-container">
        <div class="moon"></div>
        <div class="moon-glow"></div>
      </div>

      <div class="message-card">
        <h2>소원 성취</h2>
        <p>둥근 보름달처럼 풍성하고<br>환한 웃음 가득한 명절 보내세요.</p>
      </div>

      <div class="tradition-grid">
        <div class="tradition-item">
          <span class="icon">🌰</span>
          <h3>차례 (Charye)</h3>
          <p>조상님께 감사드리는<br>차례상</p>
        </div>
        <div class="tradition-item">
          <span class="icon">🥮</span>
          <h3>송편 (Songpyeon)</h3>
          <p>보름달을 닮은<br>맛있는 송편</p>
        </div>
        <div class="tradition-item">
          <span class="icon">💃</span>
          <h3>강강술래 (Ganggangsullae)</h3>
          <p>다 함께 즐기는<br>전통 놀이</p>
        </div>
      </div>

      <!-- 기능 추가: 덕담 나누기 -->
      <section class="greetings-section">
        <h2>덕담 나누기</h2>
        <form @submit.prevent="addGreeting" class="greeting-form">
          <div class="form-group">
            <label for="name">이름</label>
            <input type="text" id="name" v-model="newName" placeholder="이름을 입력하세요" required>
          </div>
          <div class="form-group">
            <label for="greeting">덕담</label>
            <textarea id="greeting" v-model="newGreeting" placeholder="따뜻한 덕담을 남겨주세요" rows="3" required></textarea>
          </div>
          <button type="submit" class="submit-btn">덕담 남기기</button>
        </form>

        <div class="greetings-list" v-if="greetings.length > 0">
          <h3>모두의 덕담</h3>
          <transition-group name="list-fade">
            <div class="greeting-card" v-for="(greeting, index) in greetings" :key="index">
              <p class="greeting-text">"{{ greeting.text }}"</p>
              <p class="greeting-author">- {{ greeting.name }} -</p>
            </div>
          </transition-group>
        </div>
      </section>

    </main>

    <footer class="app-footer">
      <p>© {{ new Date().getFullYear() }} Happy Chuseok. All rights reserved.</p>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

// 덕담 리스트를 위한 타입과 상태
interface Greeting {
  name: string;
  text: string;
}

const newName = ref<string>('');
const newGreeting = ref<string>('');
const greetings = ref<Greeting[]>([]);

// 덕담 추가 함수
const addGreeting = () => {
  if (newName.value.trim() && newGreeting.value.trim()) {
    // 새 덕담을 리스트 맨 위에 추가
    greetings.value.unshift({
      name: newName.value,
      text: newGreeting.value
    });
    // 폼 초기화
    newName.value = '';
    newGreeting.value = '';
  }
};
</script>

<style scoped>
/* Google Font 적용 */
:root, .chuseok-app {
  font-family: 'Gowun Dodum', sans-serif;
}

/* 전체 앱 스타일 */
.chuseok-app {
  min-height: 100vh;
  /* 예술적 효과: 반짝이는 별 배경 추가 */
  background:
    /* 1. Twinkling Stars */
      radial-gradient(circle, #ffffff 1px, transparent 1px),
      radial-gradient(circle, #ffffff 1px, transparent 1px),
        /* 2. Base Gradient */
      linear-gradient(170deg, #0a0a23 0%, #2a2a5e 60%, #4a4a8a 100%);
  background-size:
      300px 300px, /* stars 1 */
      200px 200px, /* stars 2 */
      100% 100%; /* base gradient */
  background-position:
      0 0,
      100px 100px,
      0 0;
  color: #ffffff;
  text-align: center;
  padding: 40px 20px;
  box-sizing: border-box;
  overflow-x: hidden; /* 가로 스크롤 방지 */
  position: relative; /* 별똥별 배치를 위함 */
  animation: twinkling 60s linear infinite;
}

/* 예술적 효과: 별똥별 */
.shooting-stars {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
}

.shooting-stars .star {
  position: absolute;
  width: 3px;
  height: 3px;
  background-color: #fffde7;
  border-radius: 50%;
  box-shadow: 0 0 10px 2px #fffde7;
  animation: shootingStar 10s linear infinite;
  opacity: 0;
}

/* 꼬리 효과 */
.shooting-stars .star::after {
  content: '';
  position: absolute;
  width: 150px;
  height: 2px;
  background: linear-gradient(to left, rgba(255, 253, 231, 0.5), transparent);
  transform: translateY(0.5px) translateX(-150px);
}

/* 별똥별 위치 및 지연시간 (예술적 효과) */
.shooting-stars .star:nth-child(1) {
  top: 10%;
  left: 100%;
  animation-delay: 0s;
}
.shooting-stars .star:nth-child(2) {
  top: 30%;
  left: 100%;
  animation-delay: 3s;
  animation-duration: 8s;
  transform: scale(0.8);
}
.shooting-stars .star:nth-child(3) {
  top: 60%;
  left: 100%;
  animation-delay: 7s;
  animation-duration: 12s;
  transform: scale(0.6);
}

/* 헤더 */
.app-header {
  margin-bottom: 40px;
  animation: fadeInDown 1s ease-out;
  position: relative; /* z-index를 주기 위해 */
  z-index: 10;
}
/* ... (기존 h1, p 스타일) ... */
.app-header h1 {
  font-size: 3rem; /* 48px */
  font-weight: bold;
  margin: 0;
  color: #fffde7; /* 약간 노란빛이 도는 흰색 */
  text-shadow: 0 0 10px rgba(255, 253, 231, 0.7);
}

.app-header p {
  font-size: 1.25rem; /* 20px */
  color: #e0e0e0;
  margin-top: 10px;
}


/* 메인 콘텐츠 */
.app-main {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 40px;
  position: relative; /* z-index를 주기 위해 */
  z-index: 10;
}

/* ... (기존 보름달, 메시지 카드, 전통 그리드 스타일) ... */
.full-moon-container {
  position: relative;
  width: 200px;
  height: 200px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 20px;
  animation: float 4s ease-in-out infinite;
}

.moon {
  width: 180px;
  height: 180px;
  background-color: #fffde7;
  border-radius: 50%;
  box-shadow: 0 0 20px #fffde7, 0 0 40px #fffde7, 0 0 60px #f5f5dc;
  /* 달 표면 질감 (선택 사항) */
  background-image: radial-gradient(circle at 30% 30%, rgba(255,255,255,0.2) 0%, rgba(255,255,255,0) 20%),
  radial-gradient(circle at 70% 60%, rgba(255,255,255,0.1) 0%, rgba(255,255,255,0) 15%);
}

.moon-glow {
  position: absolute;
  width: 220px;
  height: 220px;
  background: radial-gradient(circle, rgba(255, 253, 231, 0.15) 40%, rgba(255, 253, 231, 0) 70%);
  border-radius: 50%;
  z-index: -1;
}

.message-card {
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 12px;
  padding: 24px;
  max-width: 500px;
  width: 100%;
  box-shadow: 0 8px 32px 0 rgba(10, 10, 35, 0.37);
  backdrop-filter: blur(5px);
  -webkit-backdrop-filter: blur(5px);
  animation: fadeInUp 1s ease-out 0.5s both;
}

.message-card h2 {
  font-size: 1.75rem; /* 28px */
  margin-top: 0;
  color: #fffde7;
}

.message-card p {
  font-size: 1.1rem; /* 17.6px */
  line-height: 1.7;
  color: #e0e0e0;
}

.tradition-grid {
  display: flex;
  flex-wrap: wrap; /* 모바일에서 줄바꿈 */
  justify-content: center;
  gap: 20px;
  width: 100%;
  max-width: 800px;
  animation: fadeInUp 1s ease-out 1s both;
}

.tradition-item {
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 12px;
  padding: 20px;
  width: 200px;
  box-shadow: 0 4px 15px rgba(10, 10, 35, 0.2);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.tradition-item:hover {
  transform: translateY(-8px);
  box-shadow: 0 8px 25px rgba(255, 253, 231, 0.1);
}

.tradition-item .icon {
  font-size: 3rem; /* 48px */
  display: block;
  margin-bottom: 10px;
}

.tradition-item h3 {
  font-size: 1.25rem; /* 20px */
  margin: 10px 0;
  color: #f0f0f0;
}

.tradition-item p {
  font-size: 0.95rem; /* 15.2px */
  color: #c0c0c0;
  line-height: 1.5;
}


/* 기능 추가: 덕담 나누기 섹션 */
.greetings-section {
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 12px;
  padding: 24px;
  max-width: 600px;
  width: 100%;
  box-shadow: 0 8px 32px 0 rgba(10, 10, 35, 0.37);
  backdrop-filter: blur(5px);
  -webkit-backdrop-filter: blur(5px);
  animation: fadeInUp 1s ease-out 1.5s both;
  text-align: left;
}

.greetings-section h2 {
  text-align: center;
  font-size: 1.75rem;
  margin-top: 0;
  margin-bottom: 20px;
  color: #fffde7;
}

.greeting-form .form-group {
  margin-bottom: 15px;
}

.greeting-form label {
  display: block;
  margin-bottom: 5px;
  font-size: 0.95rem;
  color: #e0e0e0;
}

.greeting-form input[type="text"],
.greeting-form textarea {
  width: 100%;
  padding: 12px;
  background: rgba(0, 0, 0, 0.2);
  border: 1px solid rgba(255, 255, 255, 0.3);
  border-radius: 8px;
  color: #ffffff;
  font-family: 'Gowun Dodum', sans-serif;
  font-size: 1rem;
  box-sizing: border-box; /* padding이 너비에 포함되도록 */
}

.greeting-form textarea {
  resize: vertical;
}

.greeting-form input[type="text"]:focus,
.greeting-form textarea:focus {
  outline: none;
  border-color: #fffde7;
  box-shadow: 0 0 10px rgba(255, 253, 231, 0.3);
}

.submit-btn {
  width: 100%;
  padding: 12px;
  font-size: 1.1rem;
  font-weight: bold;
  color: #0a0a23;
  background: #fffde7;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s ease, box-shadow 0.3s ease;
  font-family: 'Gowun Dodum', sans-serif;
}

.submit-btn:hover {
  background-color: #f5f5dc;
  box-shadow: 0 0 15px rgba(255, 253, 231, 0.5);
}

.greetings-list {
  margin-top: 30px;
}

.greetings-list h3 {
  text-align: center;
  font-size: 1.5rem;
  color: #fffde7;
  margin-bottom: 20px;
}

.greeting-card {
  background: rgba(255, 255, 255, 0.08);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 8px;
  padding: 15px 20px;
  margin-bottom: 15px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.greeting-text {
  font-size: 1.1rem;
  line-height: 1.6;
  color: #f0f0f0;
  margin: 0 0 10px 0;
  font-style: italic;
}

.greeting-author {
  font-size: 0.95rem;
  color: #c0c0c0;
  margin: 0;
  text-align: right;
}

/* 덕담 리스트 애니메이션 */
.list-fade-enter-active,
.list-fade-leave-active {
  transition: all 0.5s ease;
}
.list-fade-enter-from,
.list-fade-leave-to {
  opacity: 0;
  transform: translateY(-10px);
}


/* 푸터 */
.app-footer {
  margin-top: 60px;
  font-size: 0.9rem; /* 14.4px */
  color: #aaa;
  animation: fadeInUp 1s ease-out 1.5s both;
  position: relative;
  z-index: 10;
}

/* 애니메이션 */
@keyframes fadeInDown {
  from { opacity: 0; transform: translateY(-30px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(30px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes float {
  0% { transform: translateY(0px); }
  50% { transform: translateY(-15px); }
  100% { transform: translateY(0px); }
}

/* 예술적 효과: 별 반짝임 */
@keyframes twinkling {
  0% { background-position: 0 0, 100px 100px, 0 0; }
  100% { background-position: -600px 600px, -500px 500px, 0 0; }
}

/* 예술적 효과: 별똥별 */
@keyframes shootingStar {
  0% {
    opacity: 0;
    transform: translateX(0) translateY(0) rotate(-45deg);
  }
  10% {
    opacity: 1;
  }
  80% {
    opacity: 0;
  }
  100% {
    opacity: 0;
    transform: translateX(-150vw) translateY(150vw) rotate(-45deg);
  }
}


/* 모바일 반응형 */
@media (max-width: 768px) {
  .app-header h1 {
    font-size: 2.5rem; /* 40px */
  }

  .app-header p {
    font-size: 1.1rem; /* 17.6px */
  }

  .tradition-grid {
    flex-direction: column;
    align-items: center;
  }

  .tradition-item {
    width: 90%;
    max-width: 300px;
  }

  .greetings-section {
    padding: 20px;
  }
}
</style>

