<script setup lang="ts">
import { invoke } from '@tauri-apps/api/tauri'
import { loginApi } from '~/apis/sys/user'

const greetMsg = ref('')
const name = ref('')

const { setPageConfig, showNotify } = usePageStore()

onShow(() => {
  setPageConfig({
    pageTitle: 'Dashboard',
  })
})

const handleTestApi = async () => {
  const res = await loginApi({ username: 'admin', password: '123456' })
  if (res.token) {
    showNotify({
      color: 'success',
      content: '登录成功',
    })
  }
}

async function greet() {
  greetMsg.value = await invoke('greet', { name: name.value })
}
</script>

<template>
  <UBasePage>
    <div class="p-6" flex="~ col gap2" justify-center>
      <AButton color="danger" block @click="showNotify({ color: 'danger', content: 'error' })">
        Show Error Notify
      </AButton>
      <AButton color="primary" block @click="showNotify({ color: 'primary', content: 'primary' })">
        Show Primary Notify
      </AButton>
      <AButton cc="bg-orange border-orange" block icon="i-carbon-notification" @click="handleTestApi">
        Custom Button
      </AButton>
    </div>
    <div class="bg-base-second m-6 rounded-lg p-6 border-base">
      <div flex="~ col gap2" justify-center>
        <input v-model="name" placeholder="Enter a name...">
        <AButton color="danger" type="button" @click="greet()">
          Greet
        </AButton>
        <p>{{ greetMsg }}</p>
      </div>
    </div>
  </UBasePage>
</template>

<style scoped>

</style>
