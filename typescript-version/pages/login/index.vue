<script setup lang="ts">
const user = useSupabaseUser()
const { auth } = useSupabaseClient()
console.log(user, 'user')
console.log(auth, 'auth')
console.log(useRuntimeConfig(), 'runtimeConfig')
const redirectTo = `${useRuntimeConfig().public.baseUrl}/confirm`

watchEffect(() => {
  if (user.value) {
    navigateTo('/')
  }
})
</script>

<template>
  <div class="min-h-full flex flex-col justify-center py-12 sm:px-6 lg:px-8">
    <h2 class="my-6 text-center text-3xl font-extrabold u-text-white">登录您的账户</h2>
    <VCard class="sm:mx-auto sm:w-full sm:max-w-md">
      <VDivider>请选择</VDivider>
      <VBtn type="primary" size="large" class="w-full"
        @click="auth.signInWithOAuth({ provider: 'github', options: { redirectTo } })">
        <VIcon name="i-simple-icons-github" class="h-5 w-5 mr-2" />
        Github
      </VBtn>
    </VCard>
  </div>
</template>