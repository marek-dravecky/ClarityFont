<template>
  <div id="app">
    <header v-if="view === 'test'" style="text-align:center; padding:20px 0;">
      <h1> ClarityFont </h1>
    </header>

    <Home v-if="view === 'home'" @start-test="goToTest" />
    <TestPage v-else @back-home="goHome" />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import Home from './components/Home.vue'
import TestPage from './components/TestPage.vue'

const view = ref('home')
function goToTest() { view.value = 'test' }
function goHome() { view.value = 'home' }
</script>
