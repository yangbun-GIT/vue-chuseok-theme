<template>
  <div class="chuseok-app">
    <!-- Google Fonts Import -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Gowun+Dodum&display=swap" rel="stylesheet">

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
    </main>

    <footer class="app-footer">
      <p>© {{ new Date().getFullYear() }} Happy Chuseok. All rights reserved.</p>
    </footer>
  </div>
</template>

<script setup lang="ts">
// 이 페이지는 정적이라 스크립트 로직이 따로 필요하지 않습니다.
// 필요시 이곳에 ref, computed 등을 추가하세요.
</script>

<style scoped>
/* Google Font 적용 */
:root, .chuseok-app {
  font-family: 'Gowun Dodum', sans-serif;
}

/* 전체 앱 스타일 */
.chuseok-app {
  min-height: 100vh;
  background: linear-gradient(170deg, #0a0a23 0%, #2a2a5e 60%, #4a4a8a 100%);
  color: #ffffff;
  text-align: center;
  padding: 40px 20px;
  box-sizing: border-box;
  overflow-x: hidden; /* 가로 스크롤 방지 */
}

/* 헤더 */
.app-header {
  margin-bottom: 40px;
  animation: fadeInDown 1s ease-out;
}

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
}

/* 보름달 */
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

/* 메시지 카드 */
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

/* 전통 아이콘 그리드 */
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

/* 푸터 */
.app-footer {
  margin-top: 60px;
  font-size: 0.9rem; /* 14.4px */
  color: #aaa;
  animation: fadeInUp 1s ease-out 1.5s both;
}

/* 애니메이션 */
@keyframes fadeInDown {
  from {
    opacity: 0;
    transform: translateY(-30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes float {
  0% {
    transform: translateY(0px);
  }
  50% {
    transform: translateY(-15px);
  }
  100% {
    transform: translateY(0px);
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
}
</style>
