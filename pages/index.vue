<script setup lang="ts">
import { createClient } from '@supabase/supabase-js'

useHead({
  title: '사주AI - 나를 아는 AI 친구',
  script: [{
    innerHTML: `!function(f,b,e,v,n,t,s)
{if(f.fbq)return;n=f.fbq=function(){n.callMethod?
n.callMethod.apply(n,arguments):n.queue.push(arguments)};
if(!f._fbq)f._fbq=n;n.push=n;n.loaded=!0;n.version='2.0';
n.queue=[];t=b.createElement(e);t.async=!0;
t.src=v;s=b.getElementsByTagName(e)[0];
s.parentNode.insertBefore(t,s)}(window, document,'script',
'https://connect.facebook.net/en_US/fbevents.js');
fbq('init', '1370494164218118');
fbq('track', 'PageView');`
  }],
  noscript: [{
    innerHTML: `<img height="1" width="1" style="display:none"
src="https://www.facebook.com/tr?id=1370494164218118&ev=PageView&noscript=1"
/>`
  }]
})

const email = ref('')
const submitted = ref(false)
const loading = ref(false)
const errorMessage = ref(' ')

const { public: { supabaseUrl, supabaseKey } } = useRuntimeConfig()
const supabase = createClient(supabaseUrl, supabaseKey)

const submitEmail = async () => {
  // Validate email
  const emailRegex = /^[^\s@]+@[^\s @]+\.[^\s@    ]+$/
  if (!emailRegex.test(email.value)) {
    errorMessage.value = '유효한 이메일 주소를 입력해주세요.'
    return
  }

  loading.value = true
  errorMessage.value = ''

  //   Store the email in   Supabase
  const { error } = await supabase
    .from('waitlist')
    .insert([{ email: email.value }])

  if (error) {
    errorMessage.value = '이메일 저장 중 오류가 발생했습니다.'
  } else {
    // Successfully submitted
    submitted.value = true
  }

  email.value = ''
  loading.value = false
}
</script>

<template>
  <div class="landing-page">
    <header>
      <div class="logo">사주AI</div>
    </header>

    <main>
      <section class="hero">
        <h1>나도 모르는 나를, 사주로 매일 알려주는 파트너가 생겼어요.</h1>
        <p class="subtitle">매일 쌓이는 작은 고민들, 내 사주 기반 AI가 대신 답해줄게요.</p>
      </section>

      <section class="signup">
        <h2>곧 출시됩니다!</h2>
        <p>출시 소식을 가장 먼저 받아보세요.</p>

        <div class="form-container" v-if="!submitted">
          <form @submit.prevent="submitEmail" class="email-form">
            <input type="email" v-model="email" placeholder="이메일 주소를 입력하세요" required />
            <button type="submit" :disabled="loading">
              {{ loading ? '처리 중...' : '알림 받기' }}
            </button>
          </form>
          <p class="error" v-if="errorMessage">{{ errorMessage }}</p>
        </div>

        <div class="success-message" v-else>
          <p>감사합니다! 출시 소식을 이메일로 알려드리겠습니다.</p>
        </div>
      </section>

      <section class="features">
        <div class="feature-card">
          <h3>매일매일, 나만을 위한 소름 돋는 조언</h3>
        </div>

        <div class="feature-card">
          <h3>복잡한 정보 입력? 한 번이면 끝!</h3>
        </div>

        <div class="feature-card">
          <h3>오늘의 나, 내 이상형, 우리의 궁합까지</h3>
        </div>

        <div class="feature-card">
          <h3>내 마음속 이야기까지 먼저 알아주는 AI 친구</h3>
        </div>

        <div class="feature-card">
          <h3>고민 없이 하루를 가볍게 시작하는 방법</h3>
        </div>

        <div class="feature-card">
          <h3>나보다 나를 더 잘 아는 친구, 지금 만나보세요.</h3>
        </div>
      </section>
    </main>

    <footer>
      <p>&copy; 2025 사주AI. All rights reserved.</p>
    </footer>
  </div>
</template>

<style scoped>
* {
  word-break: keep-all;
}

.landing-page {
  font-family: 'Noto Sans KR', sans-serif;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
  color: #333;
  line-height: 1.6;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 0;
}

.logo {
  font-size: 24px;
  font-weight: bold;
  color: #6b46c1;
}

.hero {
  text-align: center;
  padding: 80px 0;
}

h1 {
  font-size: 2.5rem;
  margin-bottom: 20px;
  font-weight: bold;
  color: #4a5568;
}

.subtitle {
  font-size: 1.5rem;
  color: #718096;
  max-width: 800px;
  margin: 0 auto;
}

.features {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 30px;
  margin: 60px 0;
}

.feature-card {
  background-color: #f7fafc;
  border-radius: 10px;
  padding: 30px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
  transition: transform 0.3s ease;
}

.feature-card:hover {
  transform: translateY(-5px);
}

.feature-card h3 {
  font-size: 1.2rem;
  margin-bottom: 15px;
  color: #4a5568;
  font-weight: 600;
}

.signup {
  text-align: center;
  padding: 80px 0;
  background-color: #f9f7ff;
  border-radius: 10px;
  margin: 40px 0;
}

.signup h2 {
  font-size: 2rem;
  margin-bottom: 15px;
  color: #4a5568;
}

.email-form {
  display: flex;
  max-width: 500px;
  margin: 30px auto;
}

input[type="email"] {
  flex-grow: 1;
  padding: 12px 15px;
  border: 1px solid #e2e8f0;
  border-radius: 5px 0 0 5px;
  font-size: 16px;
}

button {
  padding: 12px 25px;
  background-color: #6b46c1;
  color: white;
  border: none;
  border-radius: 0 5px 5px 0;
  cursor: pointer;
  font-weight: bold;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #553c9a;
}

button:disabled {
  background-color: #a0aec0;
  cursor: not-allowed;
}

.error {
  color: #e53e3e;
  margin-top: 10px;
}

.success-message {
  color: #38a169;
  margin: 30px 0;
  font-weight: bold;
}

.cta-button {
  margin-top: 40px;
}

.button {
  display: inline-block;
  padding: 15px 30px;
  text-decoration: none;
  border-radius: 5px;
  font-weight: bold;
  transition: all 0.3s;
}

.primary {
  background-color: #6b46c1;
  color: white;
}

.primary:hover {
  background-color: #553c9a;
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

footer {
  text-align: center;
  padding: 30px 0;
  color: #a0aec0;
  margin-top: 60px;
  border-top: 1px solid #edf2f7;
}

@media (max-width: 768px) {
  h1 {
    font-size: 2rem;
  }

  .subtitle {
    font-size: 1.2rem;
  }

  .email-form {
    flex-direction: column;
  }

  input[type="email"] {
    border-radius: 5px;
    margin-bottom: 10px;
  }

  button {
    border-radius: 5px;
    width: 100%;
  }
}
</style>