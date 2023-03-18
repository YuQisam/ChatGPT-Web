<script setup lang='ts'>
import { computed, ref, watch } from 'vue'
import { NCard, NModal } from 'naive-ui'
import { fetchChatConfig } from '@/api'

interface Props {
  visible: boolean
}

interface Emit {
  (e: 'update:visible', visible: boolean): void
}

interface ConfigState {
  timeoutMs?: number
  reverseProxy?: string
  apiModel?: string
  socksProxy?: string
}

const props = defineProps<Props>()

const emit = defineEmits<Emit>()

const show = computed({
  get() {
    return props.visible
  },
  set(visible: boolean) {
    emit('update:visible', visible)
  },
})

const config = ref<ConfigState>()

async function fetchConfig() {
  try {
    const { data } = await fetchChatConfig<ConfigState>()
    config.value = data
  }
  catch (error) {
    // ...
  }
}

watch(
  () => props.visible,
  (val) => {
    if (val)
      fetchConfig()
  },
)
</script>

<template>
  <NModal v-model:show="show" style="width: 80%; max-width: 350px;">
    <NCard>
      <div class="space-y-4">
        <h2 class="text-xl font-bold text-center">
          关注公众号
          <img src="@/assets/qrcode.jpg" alt="二维码" style="width: 300px; height: 300px;">
        </h2>
        <hr>

        <h2 class="text-xl font-bold text-center">
          chatgpt微信交流群
          <img src="@/assets/wechat.jpg" alt="二维码" style="width: 300px; height: 300px;">
        </h2>
        <hr>
        <!-- <p>ID：关注公号</p>
          <p>当天已咨询次数：0</p>
          <p>历史总咨询次数：100</p>
          <p>VIP到期时间：2023-04-10 00:00:00</p> -->
      </div>
    </NCard>
  </NModal>
</template>
