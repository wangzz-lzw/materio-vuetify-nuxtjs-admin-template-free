<script setup lang="ts">
import { useTheme } from 'vuetify'

const { global } = useTheme()
const user = useSupabaseUser()
const { auth } = useSupabaseClient()
const redirectTo = `${useRuntimeConfig().app.baseURL}/confirm`

watchEffect(() => {
  if (user.value) {
    navigateTo('/dashboard')
  }
})
const authProviders = [
  {
    icon: 'bxl-github',
    color: '#272727',
    colorInDark: '#fff',
  }
]
const handleClick = (link) => {
  if (link.icon === 'bxl-github') {
    console.log('link')
    auth.signInWithOAuth({ provider: 'github', options: { redirectTo } })
  }
}
</script>

<template>
  <VBtn v-for="link in authProviders" :key="link.icon" :icon="link.icon" variant="text"
    :color="global.name.value === 'dark' ? link.colorInDark : link.color" @click="handleClick(link)" />
</template>
