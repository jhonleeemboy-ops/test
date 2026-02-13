<template>
  <div class="result">

    <!-- Score Ring -->
    <div class="result-score-wrap">
      <div class="result-score-ring" :style="{ '--pct': pct + '%' }">
        <span class="result-score-num">{{ score }}/{{ total }}</span>
      </div>
      <div class="result-title">{{ resultTitle }}</div>
      <div class="result-sub">{{ resultSub }}</div>
    </div>

    <!-- Reveal Steps -->
    <div class="reveal-steps">
      <div
        v-for="(step, i) in revealSteps"
        :key="i"
        class="reveal-step"
        :class="{ show: visibleSteps[i], highlight: step.highlight }"
      >
        <span class="reveal-step-icon">{{ step.icon }}</span>
        <span class="reveal-step-text" v-html="step.text"></span>
      </div>
    </div>

    <!-- Confession Final -->
    <div class="confession-final" :class="{ show: showConfession }">
      <span class="confession-final-emoji">💜</span>
      <p class="confession-final-text">
        Shanny Dane, I made this whole quiz just to say: I really like you.
        Like genuinely, not just a "haha jk" kind of like. 😅
        <br/><br/>
        I know it's random and lowkey chaotic but...
        will you give me a chance? 💜
      </p>
      <p class="confession-final-name">— Jhon Lee aka Haru</p>
    </div>

    <!-- Buttons -->
    <div class="btn-row">
      <button class="btn-secondary" @click="$emit('restart')">🔄 Play Again</button>
      <button class="btn btn-primary" style="flex:2;" @click="$emit('restart')">Share with a Friend 😏</button>
    </div>

  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'

const props = defineProps({
  score: Number,
  total: Number
})

defineEmits(['restart'])

const pct = computed(() => Math.round((props.score / props.total) * 100))

const titles = ['Keep Studying! 📖', 'Not Bad! 📚', 'Pretty Good! 🎯', 'Smart Cookie! 🧠', 'Quiz Master! 🏆']
const subs   = ['Room for improvement~', 'Getting there!', 'Above average!', 'Impressive!', 'Absolutely crushing it!']
const tier   = computed(() => Math.floor((props.score / props.total) * (titles.length - 1)))
const resultTitle = computed(() => titles[tier.value])
const resultSub   = computed(() => subs[tier.value])

const revealSteps = [
  { icon: '📊', text: '<strong>Accounting Knowledge:</strong> You survived 5 accounting questions. Your professors would be... slightly proud.', highlight: false, delay: 300 },
  { icon: '🎂', text: '<strong>Self Awareness:</strong> You (eventually) remembered your own birthday. That\'s reassuring.', highlight: false, delay: 900 },
  { icon: '🎮', text: '<strong>Gaming Identity:</strong> Lesley main confirmed. Sniper behavior noted.', highlight: false, delay: 1500 },
  { icon: '🤔', text: '<strong>Wait... why does this quiz know so much about you?</strong> Who made this? 👀', highlight: true, delay: 2200 },
  { icon: '💜', text: '<strong>Surprise!</strong> This wasn\'t just a trivia quiz. Someone wanted to get your attention — and apparently the only way was through accounting questions 😂', highlight: true, delay: 3100 }
]

const visibleSteps = ref(revealSteps.map(() => false))
const showConfession = ref(false)

onMounted(() => {
  revealSteps.forEach((step, i) => {
    setTimeout(() => {
      visibleSteps.value[i] = true
    }, step.delay)
  })

  setTimeout(() => {
    showConfession.value = true
    launchConfetti()
  }, 3900)
})

function launchConfetti() {
  const cols = ['#6c63ff', '#a78bfa', '#c4b5fd', '#7c3aed', '#ddd6fe', '#ffffff']
  for (let i = 0; i < 70; i++) {
    setTimeout(() => {
      const c = document.createElement('div')
      c.style.cssText = `
        position: fixed;
        left: ${Math.random() * 100}vw;
        top: -8px;
        width: ${6 + Math.random() * 7}px;
        height: ${6 + Math.random() * 7}px;
        background: ${cols[Math.floor(Math.random() * cols.length)]};
        border-radius: ${Math.random() > 0.5 ? '50%' : '3px'};
        animation: cfFall ${2 + Math.random() * 2.5}s linear forwards;
        pointer-events: none;
        z-index: 999;
      `
      document.body.appendChild(c)
      setTimeout(() => c.remove(), 5000)
    }, i * 45)
  }
}
</script>

<style scoped>
.result { animation: fadeIn 0.4s ease both; }

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to   { opacity: 1; transform: translateY(0); }
}

.result-score-wrap {
  text-align: center;
  margin-bottom: 1.8rem;
}

.result-score-ring {
  width: 110px;
  height: 110px;
  border-radius: 50%;
  background: conic-gradient(var(--accent) 0%, var(--accent2) var(--pct, 70%), var(--border) var(--pct, 70%));
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 1rem;
  position: relative;
}

.result-score-ring::before {
  content: '';
  position: absolute;
  inset: 10px;
  background: var(--card);
  border-radius: 50%;
}

.result-score-num {
  position: relative;
  z-index: 1;
  font-size: 1.6rem;
  font-weight: 800;
  color: var(--white);
}

.result-title {
  font-size: 1.35rem;
  font-weight: 800;
  color: var(--white);
  margin-bottom: 0.3rem;
}

.result-sub {
  font-size: 0.82rem;
  color: var(--muted);
}

.reveal-steps { margin-bottom: 1.5rem; }

.reveal-step {
  background: var(--card2);
  border: 1px solid var(--border);
  border-radius: 14px;
  padding: 1.1rem 1.2rem;
  margin-bottom: 0.75rem;
  font-size: 0.88rem;
  color: var(--muted);
  display: flex;
  align-items: flex-start;
  gap: 0.8rem;
  opacity: 0;
  transform: translateY(10px);
  transition: all 0.4s ease;
}

.reveal-step.show { opacity: 1; transform: translateY(0); }
.reveal-step.highlight { border-color: rgba(108,99,255,0.4); background: rgba(108,99,255,0.08); }
.reveal-step.highlight .reveal-step-text { color: var(--text); }

.reveal-step-icon { font-size: 1.1rem; flex-shrink: 0; margin-top: 0.05rem; }
.reveal-step-text { line-height: 1.55; }

.confession-final {
  background: linear-gradient(135deg, rgba(108,99,255,0.12), rgba(167,139,250,0.08));
  border: 1.5px solid rgba(108,99,255,0.35);
  border-radius: 16px;
  padding: 1.6rem 1.5rem;
  text-align: center;
  margin-bottom: 1.5rem;
  opacity: 0;
  transform: scale(0.97);
  transition: all 0.5s cubic-bezier(0.22,1,0.36,1);
}

.confession-final.show { opacity: 1; transform: scale(1); }

.confession-final-emoji {
  font-size: 2.5rem;
  display: block;
  margin-bottom: 0.8rem;
  animation: heartbeat 1.5s ease infinite;
}

@keyframes heartbeat {
  0%,100% { transform: scale(1); }
  20% { transform: scale(1.18); }
  40% { transform: scale(1); }
  60% { transform: scale(1.1); }
}

.confession-final-text {
  font-size: 0.97rem;
  font-weight: 500;
  color: var(--white);
  line-height: 1.7;
  margin-bottom: 1rem;
}

.confession-final-name {
  font-size: 0.82rem;
  color: var(--accent2);
  font-weight: 600;
}

.btn-row { display: flex; gap: 0.75rem; }

.btn-secondary {
  flex: 1;
  background: var(--card2);
  border: 1.5px solid var(--border);
  color: var(--muted);
  font-family: 'Poppins', sans-serif;
  font-size: 0.85rem;
  font-weight: 600;
  padding: 0.8rem;
  border-radius: 12px;
  cursor: pointer;
  transition: all 0.2s;
}

.btn-secondary:hover { border-color: var(--accent); color: var(--accent2); }
</style>

<style>
@keyframes cfFall {
  0%   { transform: translateY(-10px) rotate(0deg); opacity: 1; }
  100% { transform: translateY(100vh) rotate(540deg); opacity: 0; }
}
</style>