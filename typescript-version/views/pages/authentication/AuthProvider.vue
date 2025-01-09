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
    icon: 'bxl-facebook',
    color: '#4267b2',
    colorInDark: '#4267b2',
  },
  {
    icon: 'bxl-twitter',
    color: '#1da1f2',
    colorInDark: '#1da1f2',
  },
  {
    icon: 'bxl-github',
    color: '#272727',
    colorInDark: '#fff',
  },
  {
    icon: 'bxl-google',
    color: '#db4437',
    colorInDark: '#db4437',
  },
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
