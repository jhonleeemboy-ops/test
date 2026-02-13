<template>
  <div class="question" :key="index">
    <div class="q-category">{{ question.cat }}</div>
    <p class="q-text">{{ question.q }}</p>

    <div class="options">
      <button
        v-for="(opt, i) in question.opts"
        :key="i"
        class="opt"
        :class="{
          correct: answered && i === question.ans,
          wrong: wrongIndexes.includes(i)
        }"
        :disabled="answered || wrongIndexes.includes(i)"
        @click="pick(i)"
      >
        <span class="opt-key">{{ keys[i] }}</span>
        <span>{{ opt }}</span>
      </button>
    </div>

    <div v-if="feedback" class="feedback" :class="feedbackType">
      {{ feedback }}
    </div>

    <button
      v-if="answered"
      class="btn btn-primary"
      @click="$emit('next')"
    >
      {{ isLast ? 'See Your Results 🎁' : 'Next Question →' }}
    </button>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  question: Object,
  index: Number,
  total: Number
})

const emit = defineEmits(['correct', 'next'])

const keys = ['A', 'B', 'C', 'D']
const answered = ref(false)
const wrongIndexes = ref([])
const feedback = ref('')
const feedbackType = ref('')
const teaseIdx = ref(0)
const isLast = props.index === props.total - 1

// Reset state when question changes
watch(() => props.index, () => {
  answered.value = false
  wrongIndexes.value = []
  feedback.value = ''
  feedbackType.value = ''
  teaseIdx.value = 0
})

function pick(sel) {
  if (answered.value) return

  if (sel === props.question.ans) {
    answered.value = true
    feedback.value = getCorrectMsg()
    feedbackType.value = 'ok'
    emit('correct')
  } else {
    wrongIndexes.value.push(sel)
    feedback.value = '❌ ' + props.question.roast[Math.min(teaseIdx.value, props.question.roast.length - 1)]
    feedbackType.value = 'bad'
    teaseIdx.value++
  }
}

function getCorrectMsg() {
  if (props.index === 5) return '✅ Finally! You remembered your own birthday 🎂'
  if (props.index === 6) return '✅ Lesley main spotted! Now scroll down for something... unexpected 👀'
  const msgs = [
    '✅ Correct! See, you DO know accounting!',
    '✅ That\'s right! CPA arc loading... 📊',
    '✅ Nailed it! Keep going!'
  ]
  return msgs[Math.floor(Math.random() * msgs.length)]
}
</script>

<style scoped>
.question { animation: fadeIn 0.35s ease both; }

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to   { opacity: 1; transform: translateY(0); }
}

.q-category {
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  background: rgba(108, 99, 255, 0.1);
  border: 1px solid rgba(108, 99, 255, 0.25);
  color: var(--accent2);
  font-size: 0.68rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  padding: 0.28rem 0.8rem;
  border-radius: 99px;
  margin-bottom: 1.1rem;
}

.q-text {
  font-size: 1.1rem;
  font-weight: 600;
  color: var(--white);
  line-height: 1.55;
  margin-bottom: 1.6rem;
}

.options {
  display: flex;
  flex-direction: column;
  gap: 0.65rem;
  margin-bottom: 1rem;
}

.opt {
  display: flex;
  align-items: center;
  gap: 0.85rem;
  padding: 0.85rem 1rem;
  background: var(--card2);
  border: 1.5px solid var(--border);
  border-radius: 12px;
  cursor: pointer;
  font-family: 'Poppins', sans-serif;
  font-size: 0.88rem;
  font-weight: 500;
  color: var(--text);
  transition: all 0.18s ease;
  text-align: left;
  width: 100%;
}

.opt:hover:not(:disabled):not(.wrong) {
  border-color: var(--accent);
  background: rgba(108, 99, 255, 0.08);
  color: var(--white);
}

.opt-key {
  min-width: 26px;
  height: 26px;
  border-radius: 8px;
  background: var(--border);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.72rem;
  font-weight: 800;
  color: var(--muted);
  transition: all 0.18s;
  flex-shrink: 0;
}

.opt.correct { background: rgba(34,197,94,0.1); border-color: var(--green); color: #86efac; }
.opt.correct .opt-key { background: var(--green); color: white; }
.opt.wrong { background: rgba(239,68,68,0.1); border-color: var(--red); color: #fca5a5; }
.opt.wrong .opt-key { background: var(--red); color: white; }
.opt:disabled { cursor: not-allowed; opacity: 0.8; }

.feedback {
  border-radius: 10px;
  padding: 0.8rem 1rem;
  font-size: 0.85rem;
  font-weight: 500;
  line-height: 1.5;
  margin-bottom: 1rem;
  animation: popIn 0.25s ease;
}

@keyframes popIn {
  from { opacity: 0; transform: scale(0.96); }
  to   { opacity: 1; transform: scale(1); }
}

.feedback.ok { background: rgba(34,197,94,0.1); color: #86efac; border: 1px solid rgba(34,197,94,0.25); }
.feedback.bad { background: rgba(239,68,68,0.08); color: #fca5a5; border: 1px solid rgba(239,68,68,0.2); }
</style>