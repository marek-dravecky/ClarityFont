<template>
  <section class="test-page">
    <div class="container">
          <div class="header-row">
            <h2 style="margin:0 auto">🕒 Timed Reading Test</h2>
          </div>

      <p>Read one paragraph at a time. Click 'Start Reading' to begin the timer, then 'Done Reading' when finished.</p>

      <div class="status" v-html="statusMessage"></div>

      <div class="text-display" v-html="currentText" v-if="!state.finished"></div>

      <div class="controls">
        <button v-if="!state.finished" class="primary" @click="toggleTimer">{{ buttonLabel }}</button>
        <button class="reset-btn" @click="goHome">Reset</button>
      </div>

      <div v-if="state.finished" class="results" v-html="resultsHtml"></div>
    </div>
  </section>
</template>

<script setup>
import { ref, reactive, computed } from 'vue'
const emit = defineEmits(['back-home'])

const fonts = ["OpenDyslexic", "Arial", "Verdana", "Comic Sans MS"]
const sampleText = "Reading is a skill that helps us learn, explore, and connect with the world. For some people, reading can be challenging, especially when letters seem to move or mix together. Finding the right font can make a big difference by improving comfort, focus, and speed. The goal is not to read faster, but to read with less effort and more understanding."

const state = reactive({ fontIndex: 0, running: false, startTime: null, finished: false })
const userTimes = reactive({})
const statusMessage = ref('')
const buttonLabel = ref('Start Reading')

const currentText = computed(() => {
  if (state.finished) return ''
  const f = fonts[state.fontIndex]
  return `<p style="font-family:${f}; font-size:22px;">${sampleText}</p>`
})

function toggleTimer() {
  if (!state.running) {
    state.startTime = Date.now()
    state.running = true
    buttonLabel.value = 'Done Reading'
    statusMessage.value = '⏳ Timer started. Read the text and click \"Done Reading\" when finished.'
    return
  }

  // stopping
  if (!state.startTime) {
    statusMessage.value = 'You need to start the timer first.'
    return
  }
  const elapsed = Math.round(((Date.now() - state.startTime) / 1000) * 100) / 100
  const fontName = fonts[state.fontIndex]
  userTimes[fontName] = elapsed

  // advance
  state.fontIndex += 1
  state.running = false
  state.startTime = null
  buttonLabel.value = 'Start Reading'

  if (state.fontIndex < fonts.length) {
    const nextFont = fonts[state.fontIndex]
    statusMessage.value = `✅ You read ${fontName} in ${elapsed} seconds. Next font: ${nextFont}`
  } else {
    // finished
    state.finished = true
    statusMessage.value = `🏁 All done! Results below.`
  }
}

function resetApp() {
  for (const k of Object.keys(userTimes)) delete userTimes[k]
  state.fontIndex = 0
  state.running = false
  state.startTime = null
  state.finished = false
  buttonLabel.value = 'Start Reading'
  statusMessage.value = "App reset. Click 'Start Reading' to begin!"
}

function goHome() {
  // Reset state locally and navigate back to home
  resetApp()
  emit('back-home')
}

const resultsHtml = computed(() => {
  if (!state.finished) return ''
  const entries = Object.entries(userTimes)
  if (entries.length === 0) return '<p>No results recorded.</p>'
  const summary = entries.map(([f, t]) => `${f}: ${t}s`).join('<br>')
  // find min
  let bestFont = entries[0][0]
  let bestTime = entries[0][1]
  for (let i = 1; i < entries.length; i++) {
    if (entries[i][1] < bestTime) { bestTime = entries[i][1]; bestFont = entries[i][0] }
  }
  return `<div>Results:<br>${summary}<br><br>💡 Your fastest font: <b>${bestFont}</b></div>`
})
</script>

<style scoped>
.container{ max-width:800px; margin:20px auto; padding:10px }
.header-row{ display:flex; align-items:center; gap:12px }
.header-row h2{ margin:0 }
.status{ margin:12px 0; font-weight:500 }
.text-display{ border:1px solid #eee; padding:18px; border-radius:8px; background:#fff }
.controls{ position:fixed; left:0; right:0; bottom:20px; display:flex; justify-content:center; gap:12px; z-index:1000 }
.primary{ background:#4f46e5; color:white; border:none; padding:12px 22px; border-radius:999px; cursor:pointer; box-shadow:0 6px 18px rgba(79,70,229,0.18) }
.reset-btn{ background:#efefef; border:1px solid #ddd; padding:10px 16px; border-radius:999px; cursor:pointer }
.results{ margin-top:18px; padding:12px; background:#f9fafb; border-radius:8px; min-height:60px }
</style>
