<script setup lang="ts">
import { ref, watch, computed, onMounted, unref } from 'vue'
import { useAppStore } from '@/store/modules/app'
import { useDesign } from '@/hooks/web/useDesign'

const { getPrefixCls } = useDesign()

const prefixCls = getPrefixCls('logo')

const appStore = useAppStore()

const show = ref(true)

const title = computed(() => appStore.getTitle)

const collapse = computed(() => appStore.getCollapse)

onMounted(() => {
  if (unref(collapse)) show.value = false
})

watch(
  () => collapse.value,
  (collapse: boolean) => {
    show.value = !collapse
  }
)
</script>

<template>
  <div>
    <router-link
      :class="[
        prefixCls,
        `${prefixCls}__Top flex !h-[var(--logo-height)] items-center cursor-pointer pl-8px relative decoration-none overflow-hidden`
      ]"
      to="/"
    >
      <img
        src="@/assets/imgs/logo.png"
        class="w-[calc(var(--logo-height)-10px)] h-[calc(var(--logo-height)-10px)]"
      />
      <div
        v-if="show"
        :class="['ml-10px text-16px font-700', 'text-[var(--logo-title-text-color)]']"
      >
        {{ title }}
      </div>
    </router-link>
  </div>
</template>
