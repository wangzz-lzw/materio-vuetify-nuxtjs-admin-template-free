<script setup lang="ts">
import { useTheme, type SubmitEventPromise } from 'vuetify'
import AuthProvider from '@/views/pages/authentication/AuthProvider.vue'

import logo from '@images/logo.svg?raw'
import authV1MaskDark from '@images/pages/auth-v1-mask-dark.png'
import authV1MaskLight from '@images/pages/auth-v1-mask-light.png'
import authV1Tree2 from '@images/pages/auth-v1-tree-2.png'
import authV1Tree from '@images/pages/auth-v1-tree.png'

interface IForm {
  email: string,
  password: string,
  remember: boolean
}

const handler = (str: string) => {
  let arr = str.split('_');

  let newArr = arr.map((ele, idx) => {

    return idx === 0 ? ele : ele[0].toUpperCase() + ele.slice(1)
  })
  return newArr.join('')
}
const accountDataLocal = ref<Record<string, any>>({})


const getUserinfo = async () => {
  return new Promise(async (resolve) => {
    const supabase = useSupabaseClient()
    const { data } = await supabase.from('user').select()
    console.log('data', data)
    Object.keys(data![0]).forEach((key: string) => {
      accountDataLocal.value[handler(key)] = data![0][key]
    })
    resolve(data)
  })

}
const form = ref<IForm>({
  email: '',
  password: '',
  remember: false,
})

const vuetifyTheme = useTheme()

const authThemeMask = computed(() => {
  return vuetifyTheme.global.name.value === 'light'
    ? authV1MaskLight
    : authV1MaskDark
})


const handleSubmit = async () => {
  const data = await getUserinfo()
  
}
const isPasswordVisible = ref(false)

definePageMeta({ layout: 'blank' })
</script>

<template>
  <!-- eslint-disable vue/no-v-html -->

  <div class="auth-wrapper d-flex align-center justify-center pa-4">
    <VCard class="auth-card pa-4 pt-7" max-width="448">
      <VCardItem class="justify-center">
        <NuxtLink to="/" class="d-flex align-center gap-3">
          <!-- eslint-disable vue/no-v-html -->
          <div class="d-flex" v-html="logo" />
          <h2 class="font-weight-medium text-2xl text-uppercase">
            Materio
          </h2>
        </NuxtLink>
      </VCardItem>

      <VCardText class="pt-2">
        <h4 class="text-h4 mb-1">
          欢迎登录 👋🏻
        </h4>
        <p class="mb-0">
          请输入你的账号和密码开启新的旅程
        </p>
      </VCardText>

      <VCardText>
        <VForm @submit.prevent="handleSubmit">
          <VRow>
            <!-- email -->
            <VCol cols="12">
              <VTextField :id="useId()" v-model="form.email" label="邮箱" type="email" />
            </VCol>

            <!-- password -->
            <VCol cols="12">
              <VTextField :id="useId()" v-model="form.password" label="密码" placeholder="············"
                :type="isPasswordVisible ? 'text' : 'password'" autocomplete="password"
                :append-inner-icon="isPasswordVisible ? 'ri-eye-off-line' : 'ri-eye-line'"
                @click:append-inner="isPasswordVisible = !isPasswordVisible" />

              <!-- remember me checkbox -->
              <div class="d-flex align-center justify-space-between flex-wrap my-6">
                <VCheckbox :id="useId()" v-model="form.remember" label="记住密码" />

                <a class="text-primary" href="javascript:void(0)">
                  忘记密码?
                </a>
              </div>

              <!-- login button -->
              <VBtn block type="submit" @click="handleSubmit">
                登录
              </VBtn>
            </VCol>

            <!-- create account -->
            <VCol cols="12" class="text-center text-base">
              <span>新用户?</span>
              <NuxtLink class="text-primary ms-2" to="/register">
                注册账号
              </NuxtLink>
            </VCol>

            <VCol cols="12" class="d-flex align-center">
              <VDivider />
              <span class="mx-2 whitespace-no-wrap">或者</span>
              <VDivider />
            </VCol>

            <!-- auth providers -->
            <VCol cols="12" class="text-center">
              <AuthProvider />
            </VCol>
          </VRow>
        </VForm>
      </VCardText>
    </VCard>

    <VImg class="auth-footer-start-tree d-none d-md-block" :src="authV1Tree" :width="250" />

    <VImg :src="authV1Tree2" class="auth-footer-end-tree d-none d-md-block" :width="350" />

    <!-- bg img -->
    <VImg class="auth-footer-mask d-none d-md-block" :src="authThemeMask" />
  </div>
</template>

<style lang="scss">
@use "@core/scss/template/pages/page-auth";
</style>
