<template>
  <div class="app">
    <div class="hearts-bg">
      <span
        v-for="h in floatingHearts"
        :key="h.id"
        class="heart-float"
        :style="h.style"
      >{{ h.char }}</span>
    </div>

    <div class="container">
      <div class="topbar">
        <span class="topbar-logo">⚡ TriviaX</span>
        <span class="topbar-badge">Free Challenge</span>
      </div>

      <div class="card">
        <!-- Progress Bar -->
        <div class="progress-wrap" v-if="screen === 'question'">
          <div class="progress-top">
            <span class="progress-label">Question {{ currentIndex + 1 }} / {{ questions.length }}</span>
            <span class="progress-score">{{ score * 10 }} pts</span>
          </div>
          <div class="progress-bar">
            <div
              class="progress-fill"
              :style="{ width: (currentIndex / questions.length * 100) + '%' }"
            ></div>
          </div>
        </div>

        <!-- Screens -->
        <QuizIntro v-if="screen === 'intro'" @start="startQuiz" />
        <QuizQuestion
          v-else-if="screen === 'question'"
          :question="questions[currentIndex]"
          :index="currentIndex"
          :total="questions.length"
          @correct="onCorrect"
          @next="nextQuestion"
        />
        <QuizResult
          v-else-if="screen === 'result'"
          :score="score"
          :total="questions.length"
          @restart="restart"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import QuizIntro from './components/QuizIntro.vue'
import QuizQuestion from './components/QuizQuestion.vue'
import QuizResult from './components/QuizResult.vue'

const screen = ref('intro')
const currentIndex = ref(0)
const score = ref(0)

const heartChars = ['♡', '♥', '🌸', '✿', '❀', '·']
const floatingHearts = Array.from({ length: 20 }, (_, i) => ({
  id: i,
  char: heartChars[Math.floor(Math.random() * heartChars.length)],
  style: {
    left: Math.random() * 100 + 'vw',
    animationDuration: (9 + Math.random() * 14) + 's',
    animationDelay: (Math.random() * 14) + 's',
    fontSize: (0.8 + Math.random() * 1.4) + 'rem'
  }
}))

const questions = [
  {
    cat: '📚 Accounting',
    q: 'What is the basic accounting equation?',
    opts: ['Assets = Liabilities + Equity', 'Revenue − Expenses = Profit', 'Cash + Receivables = Total Assets', 'Debit = Credit + Balance'],
    ans: 0,
    roast: [
      'Yikes... and you\'re studying Accountancy? 💀',
      'Bestie. DAE. Assets = Liabilities + Equity. Write it 100 times. 📝',
      'Okay at this point just Google it lmao 😭'
    ]
  },
  {
    cat: '📚 Accounting',
    q: 'Which financial statement reports revenues and expenses over a specific period?',
    opts: ['Balance Sheet', 'Cash Flow Statement', 'Income Statement', 'Statement of Changes in Equity'],
    ans: 2,
    roast: [
      'Nope! Think about where Net Income shows up 👀',
      'Still wrong?? Hint: it shows if the company made money or not 💸',
      'It\'s the Income Statement bestie. Please write that down. 😭'
    ]
  },
  {
    cat: '📚 Accounting',
    q: 'In accounting, what does a \'debit\' entry represent?',
    opts: ['Always an increase in value', 'Always a decrease in value', 'An entry recorded on the left side', 'Money owed to a creditor'],
    ans: 2,
    roast: [
      'Wrong! Debit isn\'t always an increase — depends on the account type 🤔',
      'Still nope! Think T-accounts. Left side = ? 📊',
      'Left side! It\'s just the LEFT side of an account. Come on!! 😂'
    ]
  },
  {
    cat: '📚 Accounting',
    q: 'Which of the following is classified as a liability?',
    opts: ['Cash on Hand', 'Accounts Payable', 'Office Equipment', 'Retained Earnings'],
    ans: 1,
    roast: [
      'Not quite! A liability is something the business OWES 🙈',
      'Nope! Hint: the answer literally has \'Payable\' in it 👀',
      'ACCOUNTS PAYABLE. Payable = you owe it. Simple! 😂'
    ]
  },
  {
    cat: '📚 Accounting',
    q: 'What is the main purpose of preparing a Trial Balance?',
    opts: ['To compute income tax', 'To confirm total debits equal total credits', 'To calculate gross profit', 'To record adjusting entries'],
    ans: 1,
    roast: [
      'Wrong! It\'s about checking if the books are balanced ⚖️',
      'Think equality — debits and credits should match! 🤔',
      'It checks that debits = credits. That\'s literally it 😅 Try again!'
    ]
  },
  {
    cat: '🎂 Personal',
    q: 'Okay quick break from accounting — when is YOUR birthday?',
    opts: ['July 18', 'August 18', 'September 18', 'October 18'],
    ans: 1,
    roast: [
      '...you don\'t know your own birthday?? 💀',
      'BRO. It\'s YOUR birthday. How are you getting this wrong 😭😂',
      'I CANNOT. It\'s August 18. YOUR birthday. August. 18. 😂💀'
    ]
  },
  {
    cat: '🎮 Gaming',
    q: 'Last question! Who is your go-to hero in Mobile Legends?',
    opts: ['Kagura', 'Lunox', 'Lesley', 'Lancelot'],
    ans: 2,
    roast: [
      'Really? That\'s not your main 🎮',
      'Come on, you know who you always pick 👀',
      'It\'s Lesley! Don\'t lie to the quiz 😂'
    ]
  }
]

function startQuiz() {
  currentIndex.value = 0
  score.value = 0
  screen.value = 'question'
}

function onCorrect() {
  score.value++
}

function nextQuestion() {
  currentIndex.value++
  if (currentIndex.value >= questions.length) {
    screen.value = 'result'
  }
}

function restart() {
  screen.value = 'intro'
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700;800&display=swap');

*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

:root {
  --bg: #0f1117;
  --card: #1a1d27;
  --card2: #21253a;
  --border: #2e3250;
  --accent: #6c63ff;
  --accent2: #a78bfa;
  --green: #22c55e;
  --red: #ef4444;
  --text: #e8eaf6;
  --muted: #8b92b8;
  --white: #ffffff;
}

body {
  min-height: 100vh;
  background: var(--bg);
  font-family: 'Poppins', sans-serif;
  color: var(--text);
}

.app {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1.5rem;
  position: relative;
  overflow-x: hidden;
}

.hearts-bg {
  position: fixed;
  inset: 0;
  pointer-events: none;
  overflow: hidden;
  z-index: 0;
}

.heart-float {
  position: absolute;
  opacity: 0.07;
  animation: floatUp linear infinite;
}

@keyframes floatUp {
  0%   { transform: translateY(100vh) rotate(0deg); opacity: 0; }
  10%  { opacity: 0.07; }
  90%  { opacity: 0.07; }
  100% { transform: translateY(-10vh) rotate(360deg); opacity: 0; }
}

.container {
  width: 100%;
  max-width: 540px;
  z-index: 1;
  position: relative;
}

.topbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 1.5rem;
}

.topbar-logo {
  font-size: 0.78rem;
  font-weight: 700;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: var(--accent2);
  opacity: 0.7;
}

.topbar-badge {
  background: rgba(108, 99, 255, 0.15);
  border: 1px solid rgba(108, 99, 255, 0.3);
  color: var(--accent2);
  font-size: 0.7rem;
  font-weight: 600;
  padding: 0.25rem 0.75rem;
  border-radius: 99px;
}

.card {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: 20px;
  padding: 2.2rem 2rem;
  animation: slideUp 0.5s cubic-bezier(0.22, 1, 0.36, 1) both;
}

@keyframes slideUp {
  from { opacity: 0; transform: translateY(24px); }
  to   { opacity: 1; transform: translateY(0); }
}

.progress-wrap { margin-bottom: 2rem; }

.progress-top {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 0.6rem;
}

.progress-label {
  font-size: 0.72rem;
  font-weight: 600;
  color: var(--muted);
  letter-spacing: 0.06em;
  text-transform: uppercase;
}

.progress-score {
  font-size: 0.72rem;
  font-weight: 700;
  color: var(--accent2);
}

.progress-bar {
  height: 5px;
  background: var(--border);
  border-radius: 99px;
  overflow: hidden;
}

.progress-fill {
  height: 100%;
  background: linear-gradient(90deg, var(--accent), var(--accent2));
  border-radius: 99px;
  transition: width 0.5s cubic-bezier(0.22, 1, 0.36, 1);
}

.btn {
  display: block;
  width: 100%;
  padding: 0.9rem 1.5rem;
  border: none;
  border-radius: 12px;
  font-family: 'Poppins', sans-serif;
  font-size: 0.95rem;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.2s ease;
  text-align: center;
}

.btn-primary {
  background: linear-gradient(135deg, var(--accent), #8b5cf6);
  color: white;
  box-shadow: 0 4px 20px rgba(108, 99, 255, 0.35);
}

.btn-primary:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 28px rgba(108, 99, 255, 0.45);
}

.fade-enter-active, .fade-leave-active { transition: opacity 0.3s ease, transform 0.3s ease; }
.fade-enter-from { opacity: 0; transform: translateY(10px); }
.fade-leave-to { opacity: 0; transform: translateY(-10px); }

@media (max-width: 400px) {
  .card { padding: 1.8rem 1.3rem; }
}
</style>