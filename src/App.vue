<template>
  <div id="app-container">
    <!-- 별똥별을 위한 요소들 -->
    <div class="shooting-star" style="left: 20%; top: 10%; animation-delay: 0s;"></div>
    <div class="shooting-star" style="left: 40%; top: 20%; animation-delay: 3s;"></div>
    <div class="shooting-star" style="left: 70%; top: 15%; animation-delay: 5s;"></div>
    <div class="shooting-star" style="left: 90%; top: 25%; animation-delay: 8s;"></div>

    <!-- 밤하늘 그라데이션 배경 및 별 효과 -->
    <div id="stars"></div>
    <div id="stars2"></div>
    <div id="stars3"></div>

    <!-- 메인 콘텐츠 -->
    <main class="content-wrapper">
      <!-- 헤더 -->
      <header class="app-header">
        <h1 class="title">풍요로운 한가위</h1>
        <p class="subtitle">"더도 말고 덜도 말고 한가위만 같아라"</p>
        <div class="moon-container">
          <div class="moon"></div>
        </div>
      </header>

      <!-- 소원 카드 -->
      <section class="card-glass">
        <h2>소원 성취</h2>
        <p>둥근 보름달처럼 풍성하고, 굴러가는 엽전처럼 넉넉한 행복이 당신의 삶에 가득 차오르기를 기원합니다.</p>
      </section>

      <!-- 전통 놀이/음식 소개 -->
      <section class="tradition-grid">
        <div class="card-glass item">
          <span class="card-icon">🏮</span>
          <h3>차례 (Charye)</h3>
          <p>조상님께 감사드리는<br>차례상</p>
        </div>
        <div class="card-glass item">
          <span class="card-icon">🍡</span>
          <h3>송편 (Songpyeon)</h3>
          <p>오밀조밀 예쁘게 빚은<br>맛있는 송편</p>
        </div>
        <div class="card-glass item">
          <span class="card-icon">💃</span>
          <h3>강강술래 (Ganggangsullae)</h3>
          <p>다 함께 즐기는<br>전통 원무</p>
        </div>
      </section>

      <!-- 덕담 나누기 섹션 -->
      <section class="card-glass">
        <h2>덕담 나누기</h2>
        <form @submit.prevent="submitWish" class="wish-form">
          <div class="form-group">
            <input type="text" v-model="newWish.name" placeholder="이름" required />
            <input type="text" v-model="newWish.message" placeholder="따뜻한 덕담 한마디를 남겨주세요." required />
          </div>
          <button type="submit" class="submit-btn">덕담 남기기</button>
        </form>

        <ul class="wish-list">
          <li v-for="wish in wishes" :key="wish.id" class="wish-item">
            <strong>{{ wish.name }}:</strong>
            <span>{{ wish.message }}</span>
          </li>
        </ul>
      </section>
    </main>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';

interface Wish {
  id: number;
  name: string;
  message: string;
}

const newWish = ref({ name: '', message: '' });
const wishes = ref<Wish[]>([
  { id: 1, name: '관리자', message: '모두 행복하고 풍성한 한가위 보내세요!' }
]);

const submitWish = () => {
  if (newWish.value.name && newWish.value.message) {
    wishes.value.unshift({
      id: Date.now(),
      name: newWish.value.name,
      message: newWish.value.message,
    });
    newWish.value.name = '';
    newWish.value.message = '';
  }
};

// onMounted 훅을 사용하여 컴포넌트 마운트 시 폰트 로드
onMounted(() => {
  const link = document.createElement('link');
  link.href = 'https://fonts.googleapis.com/css2?family=Gowun+Dodum&display=swap';
  link.rel = 'stylesheet';
  link.setAttribute('crossorigin', 'anonymous'); // TS 오류 및 CORS 정책 대응
  document.head.appendChild(link);
});
</script>

<style scoped lang="scss">
/*
 * :global() 선택자
 * 이 컴포넌트의 스타일이면서 동시에 'body' 태그(앱 바깥 영역)에도
 * 스타일을 적용합니다.
 */
:global(html) {
  /* 스크롤바가 생겼다 사라질 때 레이아웃이 흔들리는 것을 방지 */
  overflow-y: scroll;
}

:global(body) {
  margin: 0;
  padding: 0;
  /* 기본 배경색: 앱의 가장 어두운 색과 동일하게 설정 */
  background-color: #0b001a;
  color: #fff;
  font-family: 'Gowun Dodum', sans-serif;

  /*
   * 가상 요소를 사용하여 body 전체에 별이 반짝이는
   * 'twinkling' 효과를 줍니다.
   * position: fixed로 설정하여 스크롤과 무관하게 고정됩니다.
   */
  &::before {
    content: '';
    position: fixed;
    top: 0;
    left: 0;
    width: 200vw; /* 200% * 200% 크기 */
    height: 200vh;
    /* * SVG를 인라인으로 사용하여 작은 점(별)을 생성합니다.
     * 외부 이미지 요청 없이 깔끔하게 처리됩니다.
     */
    background: transparent url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" width="4" height="4"><circle cx="1" cy="1" r="1" fill="white"/><circle cx="3" cy="3" r="1" fill="white"/></svg>') repeat;
    background-size: 400px 400px; /* 별 밀도 조절 */
    animation: twinkling 60s linear infinite;
    z-index: 0; /* 모든 콘텐츠보다 뒤에 위치 */
    opacity: 0.3;
  }
}

/*
 * 앱의 메인 컨테이너
 * body의 배경(고정된 별) 위에 z-index: 1로 배치됩니다.
 */
#app-container {
  position: relative; /* z-index가 작동하도록 position 설정 */
  z-index: 1; /* body::before(z-index: 0) 보다 위에 오도록 설정 */
  min-height: 100vh; /* 최소 화면 높이만큼 채움 */

  /* 앱 내부에 그라데이션 배경과 별 효과 추가 */
  background: linear-gradient(
          180deg,
          #0b001a 0%,
          #1f0033 30%,
          #3b004a 70%,
          #57005c 100%
  );
  overflow: hidden; /* 별똥별이 밖으로 나가지 않도록 */
}

.content-wrapper {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
  position: relative; /* z-index 적용 */
  z-index: 20; /* 별, 보름달보다 위에 위치 */
}

/* --- 헤더 및 보름달 --- */
.app-header {
  text-align: center;
  padding: 4rem 0;
  position: relative;
}

.title {
  font-size: 3rem;
  font-weight: 600;
  color: #fff;
  text-shadow: 0 0 10px #ffeea4, 0 0 20px #ffeea4;
  margin: 0;
}

.subtitle {
  font-size: 1.25rem;
  color: #e0e0e0;
  margin-top: 0.5rem;
}

.moon-container {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 150px;
  height: 150px;
  z-index: 10; /* 콘텐츠(20)보다 뒤, 배경(1)보다 앞 */
  animation: float 6s ease-in-out infinite;
}

.moon {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  background-color: #fcfade;
  box-shadow: 0 0 30px #fcfade, 0 0 60px #fcfade, 0 0 100px #f5deb3,
  inset -10px 10px 20px rgba(0, 0, 0, 0.1);
}

/* --- 유리질 카드 스타일 --- */
.card-glass {
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(10px);
  border-radius: 16px;
  padding: 1.5rem 2rem;
  margin-bottom: 2rem;
  border: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow: 0 8px 32px 0 rgba(11, 0, 26, 0.37);
  text-align: center;
}

/* --- 전통 소개 그리드 --- */
.tradition-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1.5rem;
  margin-bottom: 2rem;

  .item {
    padding: 2rem 1rem;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-start;
  }

  .card-icon {
    font-size: 2.5rem;
    margin-bottom: 1rem;
  }

  h3 {
    font-size: 1.1rem;
    font-weight: 600;
    margin: 0.5rem 0;
  }

  p {
    font-size: 0.9rem;
    color: #dcdcdc;
    margin: 0;
    line-height: 1.4;
  }
}

/* --- 덕담 나누기 폼 --- */
.wish-form {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  margin-top: 1.5rem;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;

  @media (min-width: 600px) {
    flex-direction: row;

    input[type="text"]:first-child {
      flex: 1; /* 이름 */
    }
    input[type="text"]:last-child {
      flex: 3; /* 덕담 내용 */
    }
  }
}

input[type="text"] {
  width: 100%; /* 모바일 기본값 */
  padding: 0.75rem 1rem;
  border: 1px solid rgba(255, 255, 255, 0.3);
  background: rgba(255, 255, 255, 0.1);
  border-radius: 8px;
  color: #fff;
  font-family: 'Gowun Dodum', sans-serif;
  font-size: 1rem;
  box-sizing: border-box; /* 패딩 포함 */

  &::placeholder {
    color: #ccc;
  }

  &:focus {
    outline: none;
    border-color: #ffeea4;
    box-shadow: 0 0 10px rgba(255, 238, 164, 0.3);
  }
}

.submit-btn {
  padding: 0.75rem 1rem;
  border: none;
  border-radius: 8px;
  background-color: #ffeea4;
  color: #3b004a;
  font-family: 'Gowun Dodum', sans-serif;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;

  &:hover {
    background-color: #fcfade;
    box-shadow: 0 0 15px rgba(255, 238, 164, 0.5);
  }
}

/* --- 덕담 리스트 --- */
.wish-list {
  list-style: none;
  padding: 0;
  margin-top: 2rem;
  max-height: 300px;
  overflow-y: auto;
  text-align: left;
}

.wish-item {
  background: rgba(255, 255, 255, 0.05);
  padding: 0.75rem 1.25rem;
  border-radius: 8px;
  margin-bottom: 0.5rem;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);

  strong {
    color: #ffeea4;
    margin-right: 0.5em;
  }
  span {
    color: #f0f0f0;
  }
}

/* --- 애니메이션 --- */

/* 보름달 플로팅 */
@keyframes float {
  0%, 100% {
    transform: translate(-50%, -52%);
  }
  50% {
    transform: translate(-50%, -48%);
  }
}

/* 별똥별 */
.shooting-star {
  position: absolute;
  width: 2px;
  height: 80px;
  background: linear-gradient(to top, rgba(255, 255, 255, 0) 0%, #ffffff 100%);
  border-radius: 50%;
  opacity: 0;
  transform: rotate(-45deg) translate(0, -100px);
  animation: shootingStar 5s linear infinite;
  z-index: 5; /* 보름달(10)보다 뒤, 배경(1)보다 앞 */
}

@keyframes shootingStar {
  0% {
    opacity: 0;
    transform: rotate(-45deg) translate(0, -100vh);
  }
  20% {
    opacity: 1;
  }
  100% {
    opacity: 0;
    transform: rotate(-45deg) translate(0, 100vh);
  }
}

/* 별 반짝임 (글로벌) */
@keyframes twinkling {
  0% {
    transform: translate(0, 0);
  }
  100% {
    transform: translate(-100vw, -100vh);
  }
}

/* 별 반짝임 (컴포넌트 내부 - 스크롤됨) */
@mixin star-animation($duration) {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: transparent;
  animation: star-anim $duration linear infinite;
  opacity: 0.7;
  z-index: 2; /* #app-container의 배경 */
}

#stars {
  @include star-animation(50s);
  background-image: radial-gradient(1px 1px at 20px 30px, #eee, #0000),
  radial-gradient(1px 1px at 40px 70px, #fff, #0000),
  radial-gradient(1px 1px at 50px 160px, #ddd, #0000),
  radial-gradient(1px 1px at 90px 40px, #fff, #0000),
  radial-gradient(1px 1px at 130px 80px, #fff, #0000),
  radial-gradient(1px 1px at 160px 120px, #ddd, #0000);
}
#stars2 {
  @include star-animation(100s);
  background-image: radial-gradient(1px 1px at 40px 40px, #eee, #0000),
  radial-gradient(1px 1px at 80px 120px, #fff, #0000),
  radial-gradient(1px 1px at 120px 200px, #ddd, #0000),
  radial-gradient(1px 1px at 180px 80px, #fff, #0000),
  radial-gradient(1px 1px at 260px 160px, #fff, #0000),
  radial-gradient(1px 1px at 320px 240px, #ddd, #0000);
}
#stars3 {
  @include star-animation(150s);
  background-image: radial-gradient(1px 1px at 60px 60px, #eee, #0000),
  radial-gradient(1px 1px at 120px 180px, #fff, #0000),
  radial-gradient(1px 1px at 180px 300px, #ddd, #0000),
  radial-gradient(1px 1px at 270px 120px, #fff, #0000),
  radial-gradient(1px 1px at 390px 240px, #fff, #0000),
  radial-gradient(1px 1px at 480px 360px, #ddd, #0000);
}

/* 스크롤되는 별 애니메이션 */
@keyframes star-anim {
  from {
    transform: translateY(0px);
  }
  to {
    transform: translateY(-2000px);
  }
}
</style>

