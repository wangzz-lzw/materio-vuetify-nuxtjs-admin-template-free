<script setup lang="ts">
import MarkDown from '@core/components/markDown/index.vue'
const text = ref('')
let faviconLink;

onMounted(() => {
  document.querySelectorAll('link[rel="icon"]').forEach((link) => {
    document.head.removeChild(link)
  })
  const link = document.createElement('link')
  link.rel = 'icon'
  link.href = '/logo.png'
  document.head.appendChild(link)
})
</script>

<template>
  <VRow class="match-height">
    <VCol cols="24">
      <MarkDown v-model="text" />
    </VCol>
  </VRow>
</template>
