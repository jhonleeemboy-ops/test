<template>
  <div class="intro">
    <div class="intro-header">
      <div class="intro-icon">🧠</div>
      <div class="intro-header-text">
        <h1>The Ultimate Trivia Challenge</h1>
        <p>Test your knowledge across multiple topics</p>
      </div>
    </div>

    <div class="info-grid">
      <div class="info-item">
        <div class="info-item-label">Questions</div>
        <div class="info-item-value">7 Questions</div>
      </div>
      <div class="info-item">
        <div class="info-item-label">Difficulty</div>
        <div class="info-item-value">Mixed 🎯</div>
      </div>
      <div class="info-item">
        <div class="info-item-label">Category</div>
        <div class="info-item-value">General</div>
      </div>
      <div class="info-item">
        <div class="info-item-label">Points</div>
        <div class="info-item-value">10 per answer</div>
      </div>
    </div>

    <div class="rules">
      <div class="rules-title">📋 How it works</div>
      <ul>
        <li>Pick the correct answer from 4 choices</li>
        <li>Wrong answers? You can retry until you get it</li>
        <li>A special result awaits at the end</li>
      </ul>
    </div>

    <button class="btn btn-primary" @click="$emit('start')">Start Challenge →</button>
  </div>
</template>

<script setup>
defineEmits(['start'])
</script>

<style scoped>
.intro { animation: fadeIn 0.4s ease both; }

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to   { opacity: 1; transform: translateY(0); }
}

.intro-header {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin-bottom: 1.8rem;
}

.intro-icon {
  width: 52px;
  height: 52px;
  background: linear-gradient(135deg, var(--accent), var(--accent2));
  border-radius: 14px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.5rem;
  flex-shrink: 0;
}

.intro-header-text h1 {
  font-size: 1.2rem;
  font-weight: 800;
  color: var(--white);
  line-height: 1.2;
}

.intro-header-text p {
  font-size: 0.75rem;
  color: var(--muted);
  margin-top: 0.2rem;
}

.info-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0.75rem;
  margin-bottom: 1.8rem;
}

.info-item {
  background: var(--card2);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 0.9rem 1rem;
}

.info-item-label {
  font-size: 0.65rem;
  font-weight: 600;
  color: var(--muted);
  text-transform: uppercase;
  letter-spacing: 0.08em;
  margin-bottom: 0.3rem;
}

.info-item-value {
  font-size: 0.92rem;
  font-weight: 700;
  color: var(--text);
}

.rules {
  background: rgba(108, 99, 255, 0.07);
  border: 1px solid rgba(108, 99, 255, 0.2);
  border-radius: 12px;
  padding: 1rem 1.1rem;
  margin-bottom: 1.8rem;
}

.rules-title {
  font-size: 0.7rem;
  font-weight: 700;
  color: var(--accent2);
  text-transform: uppercase;
  letter-spacing: 0.08em;
  margin-bottom: 0.6rem;
}

.rules li {
  font-size: 0.82rem;
  color: var(--muted);
  list-style: none;
  padding: 0.2rem 0;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.rules li::before { content: '→'; color: var(--accent); font-weight: 700; }
</style>