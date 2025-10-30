<template>
  <section class="front-page">
    <div class="content">
      <h1 style="text-align:center;">ClarityFont</h1>
      <h3 style="text-align:center;">Let's find out what you need to read the best</h3>
      <p class="lead">Reading should not feel like a struggle. ClarityFont is a small experiment designed to explore how different fonts can change the reading experience for people with dyslexia or reading difficulties. By testing one paragraph at a time, you will discover which font helps you read with the most comfort and focus.</p>
      <p style="text-align:center; font-size:14px; color:gray; margin-top:30px;">Created by <b>Sofia Marta Dravecka</b> | A TKS Project</p>
      <button id="start-btn" @click="$emit('start-test')">Start the Test</button>
    </div>
  </section>
</template>

<script setup>
</script>

<style scoped>
.front-page .content{ max-width:720px; margin:40px auto; padding:10px; }
.lead{ text-align:center; max-width:600px; margin:20px auto; font-size:16px }
#start-btn{ display:block; margin:20px auto; font-size:18px; background-color:#4f46e5; color:white; border-radius:12px; padding:12px 28px; border:none; cursor:pointer; transition:0.2s }
#start-btn:hover{ background-color:#3730a3 }
</style>
