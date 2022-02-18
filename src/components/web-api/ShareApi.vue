<template lang="pug">
h3 Share Api
button Share
.result
</template>

<script setup>
/**
 * @description Web Share Api
 * @link https://developer.mozilla.org/en-US/docs/Web/API/Web_Share_API
 */
import { onMounted } from 'vue'

onMounted(() => {
  const btn = document.querySelector('button')
  const resultPara = document.querySelector('.result')

  // Share must be triggered by "user activation"
  btn.addEventListener('click', async () => {
    try {
      const shareData = {
        title: 'MDN',
        text: 'Learn web development on MDN!',
        url: 'https://developer.mozilla.org',
      }
      const canShare = navigator.canShare(shareData)

      if (canShare) {
        await navigator.share(shareData)
        resultPara.textContent = 'MDN shared successfully'
      } else {
        resultPara.textContent = 'Data can not be shared'
      }
    } catch (err) {
      resultPara.textContent = 'Error: ' + err
    }
  })
})
</script>
